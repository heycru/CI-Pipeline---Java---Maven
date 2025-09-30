pipeline {
  agent any
  tools {
    jdk 'jdk17'
    maven 'maven-3.9'
  }
  options {
    timestamps()
    skipDefaultCheckout(true)   // avoid the implicit "Declarative: Checkout SCM"
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Diagnose workspace') {
      steps {
        sh '''
          echo "PWD: $(pwd)"
          echo "Workspace content (top-level):"
          ls -la

          echo "Searching for pom.xml (depth<=3):"
          find . -maxdepth 3 -name pom.xml -print
        '''
      }
    }

    stage('Locate POM') {
      steps {
        script {
          def pomPath = sh(returnStdout: true, script: "find . -maxdepth 3 -name pom.xml | head -n1").trim()
          if (!pomPath) {
            error "No pom.xml found under workspace. Verify your repo layout and commit."
          }
          env.POM = pomPath
          env.POM_DIR = sh(returnStdout: true, script: "dirname \"${pomPath}\"").trim()
          echo "Using POM: ${env.POM} (dir: ${env.POM_DIR})"
        }
      }
    }

    stage('Build & Test') {
      steps {
        sh 'mvn -B -DskipTests=false -f "$POM" clean verify'
      }
      post {
        always {
          // Publish test results regardless of exact module path
          junit '**/target/surefire-reports/*.xml'
        }
      }
    }

    stage('Package & Archive') {
      steps {
        sh 'mvn -B -f "$POM" package'
        // Archive any jars produced anywhere under target/
        archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
      }
    }
  }
}
