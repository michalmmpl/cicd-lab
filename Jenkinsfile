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

      when {
   	 expression { env.GIT_BRANCH == 'origin/main' || env.BRANCH_NAME == 'main' }
      }

      steps {
        dir('devops/ansible') {
          sh """
          ansible-playbook playbooks/setup.yml -l test
	  ansible-playbook playbooks/deploy.yml -l test
          """
        }
      }
    }

    stage('Verification test environment') {
      when {
       expression { env.GIT_BRANCH == 'origin/main' || env.BRANCH_NAME == 'main' }
      }

  steps {
    script {

      echo "Sprawdzanie Spring (8080)"
      sh '''
        STATUS=$(curl -4 --noproxy '*' -s -o /dev/null -w "%{http_code}" --connect-timeout 5 http://192.168.1.102:8080/hello)
        if [ "$STATUS" -ne 200 ]; then
          echo "ERROR: Spring endpoint zwrócił $STATUS"
          exit 1
        fi
        echo "Spring OK (200)"
      '''

      echo "Sprawdzanie Tomcat (8081)"
      sh '''
        STATUS=$(curl -4 --noproxy '*' -s -o /dev/null -w "%{http_code}" --connect-timeout 5 http://192.168.1.102:8081/hello/hello)
        if [ "$STATUS" -ne 200 ]; then
          echo "ERROR: Tomcat endpoint zwrócił $STATUS"
          exit 1
        fi
        echo "Tomcat OK (200)"
      '''
    }
  }
}

  }
}