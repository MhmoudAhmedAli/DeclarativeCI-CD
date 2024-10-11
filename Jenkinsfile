def mvn
pipeline {
  agent any
    tools {
      maven 'Maven 3.9.9'
      jdk 'JAVA_HOME'
    }
  stages {
   stage ('Maven Build') {
      steps {
        script {
          mvn= tool (name: 'Maven 3.9.9', type: 'maven') + '/bin/mvn'
        }
        sh "${mvn} clean install"
      }
    }
  stage('Build Docker Image'){
    steps{
      sh 'docker build -t dileep95/dileep-spring:$BUILD_NUMBER .'
    }
  }
  stage('Docker Container'){
    steps{
      withCredentials([usernameColonPassword(credentialsId: 'docker_dileep_creds', variable: 'DOCKER_PASS')]) {
      sh 'docker push dileep95/dileep-spring:$BUILD_NUMBER'
	  sh 'docker run -d -p 8050:8050 --name SpringbootApp dileep95/dileep-spring:$BUILD_NUMBER'
    }
    }
  }  

  }
}

