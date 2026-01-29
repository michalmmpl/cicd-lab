pipeline {
  agent any
  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }
    stage('Build & Test: Spring') {
      steps {
        dir('spring') {
          sh """
            ls -la
            echo '--- SPRING: set version to 0.0.1-SNAPSHOT ---'
            mvn versions:set -DnewVersion=0.0.1-SNAPSHOT -DgenerateBackupPoms=false

            echo '--- SPRING: mvn clean package ---'
            mvn clean package

            echo '--- SPRING: mvn test ---'
            mvn test
          """
        }
      }
    }
    stage('Build & Test: Tomcat') {
      steps {
        dir('tomcat') {
          sh """
          ls -la
          """
        }
      }
    }
}