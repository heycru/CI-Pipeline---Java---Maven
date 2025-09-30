pipeline {
  agent any
  tools {
    jdk 'jdk17'         // Make sure these names exist in Jenkins → Manage Jenkins → Tools
    maven 'maven-3.9'
  }
  options { timestamps() }
  stages {
    stage('Checkout') {
      steps { checkout scm }
    }
    stage('Build & Test') {
      steps { sh 'mvn -B -DskipTests=false clean verify' }
      post {
        always { junit '**/target/surefire-reports/*.xml' }
      }
    }
    stage('Package & Archive') {
      steps {
        sh 'mvn -B package'
        archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
      }
    }
  }
}

