pipeline {
  agent any
  tools {
    jdk 'jdk17'
    maven 'maven-3.9'
  }
  options {
    timestamps()
    skipDefaultCheckout(true)   // we'll do an explicit checkout
  }

  stages {
    stage('Checkout') {
      steps { checkout scm }
    }

    stage('Build & Test') {
      steps {
        dir('app') {
          sh 'mvn -B -DskipTests=false clean verify'
        }
      }
      post {
        always {
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
