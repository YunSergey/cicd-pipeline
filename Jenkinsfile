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
          app = docker.build("${env.IMAGE_NAME}:${env.BUILD_NUMBER}")
        }

      }
    }

    stage('Push') {
      steps {
        script {
          docker.withRegistry("https://${env.DOCKER_REGISTRY}", "${env.DOCKER_CREDENTIALS_ID}") {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
          }
        }

      }
    }

  }
  environment {
    IMAGE_NAME = 'my_image'
    DOCKER_REGISTRY = 'registry.hub.docker.com'
    DOCKER_CREDENTIALS_ID = 'yunsergey'
  }
}