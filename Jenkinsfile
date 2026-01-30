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

            echo '--- SPRING: mvn clean package ---'
            mvn clean package

            echo '--- SPRING: mvn test ---'
            mvn test

	    echo '--- SPRING mvn deploy ---'
            mvn deploy -DskipTests
          """
        }
      }
    }
    stage('Build & Test: Tomcat') {
      steps {
        dir('tomcat') {
          sh """
          ls -la

            echo '--- TOMCAT: mvn clean package ---'
            mvn clean package

            echo '--- TOMCAT: mvn test ---'
            mvn test

	    echo '--- TOMCAT: mvn deploy ---'
            mvn deploy -DskipTests
          """
        }
      }
    }
    stage('Ansible test') {
      steps {
        dir('devops/ansible') {
          sh """
          ansible-playbook playbooks/setup.yml -l test
	  ansible-playbook playbooks/deploy.yml -l test
          """
        }
      }
    }

  }
}