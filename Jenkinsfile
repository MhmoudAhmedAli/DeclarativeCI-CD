def mvn
pipeline {
agent { label 'master' }
    tools {
      maven 'Maven'
      jdk 'JAVA_HOME'
    }
  stages {
   stage ('Maven Build') {
      steps {
        script {
          mvn= tool (name: 'Maven', type: 'maven') + '/bin/mvn'
        }
        cmd "${mvn} clean install"
      }
    }
  stage('Build Docker Image'){
    steps{
      bat 'docker build -t dileep95/dileep-spring:$BUILD_NUMBER .'
    }
  }
  stage('Docker Container'){
    steps{
      withCredentials([usernameColonPassword(credentialsId: 'docker_dileep_creds', variable: 'DOCKER_PASS')]) {
      bat 'docker push dileep95/dileep-spring:$BUILD_NUMBER'
	  bat 'docker run -d -p 8050:8050 --name SpringbootApp dileep95/dileep-spring:$BUILD_NUMBER'
    }
    }
  }  

  }
post {
    always {
	mail bcc: '', body: "<br>Project: ${env.JOB_NAME} <br>Build Number: ${env.BUILD_NUMBER} <br>URL: ${env.BUILD_URL}", cc: '', charset: 'UTF-8', from: '', mimeType: 'text/html', replyTo: '', subject: "Success: Project name -> ${env.JOB_NAME}", to: "mahmoudali.25@hotmail.com";
    }
    failure {
	bat 'echo "This will run only if failed"'
      mail bcc: '', body: "<br>Project: ${env.JOB_NAME} <br>Build Number: ${env.BUILD_NUMBER} <br>URL: ${env.BUILD_URL}", cc: '', charset: 'UTF-8', from: '', mimeType: 'text/html', replyTo: '', subject: "ERROR: Project name -> ${env.JOB_NAME}", to: "mahmoudali.25@hotmail.com";
    }
  }
}
