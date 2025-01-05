pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        sh '''chmod +x ./scripts/build.sh
./scripts/build.sh'''
      }
    }

    stage('Test') {
      steps {
        sh './scripts/test.sh'
      }
    }

    stage('Build a docker image') {
      steps {
        script {
          docker.build("${env.IMAGE_NAME}:${env.BUILD_NUMBER}")
        }

      }
    }

    stage('Push') {
      steps {
        script {
          docker.withRegistry('https://registry.hub.docker.com', env.DOCKER_CREDENTIALS) {
            def app = docker.image("${env.IMAGE_NAME}:${env.BUILD_NUMBER}")
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")}
          }

        }
      }

    }
    environment {
      DOCKER_CREDENTIALS = 'yunsergey'
      IMAGE_NAME = 'YunSergey/cicd-pipeline'
    }
  }