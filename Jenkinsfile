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
        sh 'docker build -t my_image .'
      }
    }

    stage('Push') {
      steps {
        script {
          docker.withRegistry('https://registry.hub.docker.com', 'docker_hub_creds_id')

          {
            my_image.push("${env.BUILD_NUMBER}")
            my_image.push("latest")
          }
        }

      }
    }

  }
  environment {
    test_name = 'test_value'
  }
}