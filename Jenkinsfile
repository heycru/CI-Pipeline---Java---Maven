pipeline {
  agent any
  tools {
    jdk 'jdk17'
    maven 'maven-3.9'
  }
  options { timestamps() }

  // Let’s avoid the auto-checkout since we do a manual one below
  options { skipDefaultCheckout(true) }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Build & Test') {
      steps {
        dir('app') {
          sh 'mvn -B -DskipTests=false clean verify'
        }
      }
      post {
        always {
          // Path is relative to workspace; tests are under app/
          junit 'app/target/surefire-reports/*.xml'
        }
      }
    }

    stage('Package & Archive') {
      steps {
        dir('app') {
          sh 'mvn -B package'
        }
        archiveArtifacts artifacts: 'app/target/*.jar', fingerprint: true
      }
    }
  }
}
