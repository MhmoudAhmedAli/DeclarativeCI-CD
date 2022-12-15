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
        sh "mvn clean install"
      }
    }
  stage('Build Docker Image'){
    steps{
      sh 'docker build -t dileep95/dileep-spring:$BUILD_NUMBER .'
    }
  }
  stage('Docker Container'){
    steps{
      withCredentials([usernamePassword(credentialsId: 'docker', passwordVariable: 'docker_pass', usernameVariable: 'docker_user')]) {
	  sh 'docker login -u ${docker_user} -p ${docker_pass}'
      	  sh 'docker push dileep95/dileep-spring:$BUILD_NUMBER'
	  sh 'docker run -d -p 8050:8050 --name SpringbootApp dileep95/dileep-spring:$BUILD_NUMBER'
    }
    }
  }  

  }
post {
    always {
	mail bcc: '', body: "<br>Project: ${env.JOB_NAME} <br>Build Number: ${env.BUILD_NUMBER} <br>URL: ${env.BUILD_URL}", cc: '', charset: 'UTF-8', from: '', mimeType: 'text/html', replyTo: '', subject: "Success: Project name -> ${env.JOB_NAME}", to: "mahmoudali.25@hotmail.com";
    }
    failure {
	sh 'echo "This will run only if failed"'
      mail bcc: '', body: "<br>Project: ${env.JOB_NAME} <br>Build Number: ${env.BUILD_NUMBER} <br>URL: ${env.BUILD_URL}", cc: '', charset: 'UTF-8', from: '', mimeType: 'text/html', replyTo: '', subject: "ERROR: Project name -> ${env.JOB_NAME}", to: "mahmoudali.25@hotmail.com";
    }
  }
}
