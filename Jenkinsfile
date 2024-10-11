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
  }
}
