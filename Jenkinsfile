pipeline {
  agent { label 'built-in' }

  tools {
    maven 'Maven'
    jdk 'JAVA_HOME'
  }

  stages {
    stage('Maven Build') {
      steps {
        script {
          def mvn = tool(name: 'Maven', type: 'maven') + '/bin/mvn'
          sh "${mvn} clean install"
        }
      }
    }

    stage('Build Docker Image') {
      steps {
        sh 'docker build -t dileep95/dileep-spring:$BUILD_NUMBER .'
      }
    }

    stage('Push Docker Image & Run Container') {
      steps {
        withCredentials([usernameColonPassword(credentialsId: 'docker_dileep_creds', variable: 'DOCKER_PASS')]) {
          sh 'docker push dileep95/dileep-spring:$BUILD_NUMBER'
          sh 'docker run -d -p 8050:8050 --name SpringbootApp dileep95/dileep-spring:$BUILD_NUMBER'
        }
      }
    }
  }

  post {
    always {
      mail(
        to: 'prithdileep@gmail.com',
        subject: "Success: Project name -> ${env.JOB_NAME}",
        body: "<br>Project: ${env.JOB_NAME} <br>Build Number: ${env.BUILD_NUMBER} <br>URL: ${env.BUILD_URL}",
        mimeType: 'text/html'
      )
    }
    failure {
      sh 'echo "This will run only if failed"'
      mail(
        to: 'prithdileep@gmail.com',
        subject: "ERROR: Project name -> ${env.JOB_NAME}",
        body: "<br>Project: ${env.JOB_NAME} <br>Build Number: ${env.BUILD_NUMBER} <br>URL: ${env.BUILD_URL}",
        mimeType: 'text/html'
      )
    }
  }
}
