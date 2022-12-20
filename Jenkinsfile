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
	  sh 'docker run -d -p 8050:8050 --name SpringbootApp dileep95/dileep-spring:$BUILD_NUMBER'
    }
  }  

  }
}
