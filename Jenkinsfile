pipeline {
  agent {
    node {
      label 'maven'
    }
  }
  environment {
    PATH = "/opt/apache-maven-3.9.6/bin:$PATH"
  }

  stages {
    stage('Build') {
      steps {
        echo "--------- Build Started -----------"
        sh 'mvn clean deploy'
        echo "--------- Build Completed -----------"
      }
    }
    stage('Test') {
      steps {
        echo "--------- Unit Test Started -----------"
        sh 'mvn surefile-report:report'
        echo "--------- Unit Test Completed -----------"
      }
    }
    stage('SonarQube analysis') {
      environment {
        scannerHome = tool 'sonar-scanner';
      }
      steps {
        withSonarQubeEnv('sonarqube-server') { // If you have configured more than one global server connection, you can specify its name
          sh "${scannerHome}/bin/sonar-scanner"
        }
      }    
    }
  }
}
