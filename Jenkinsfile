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
          def dockerImage = "mydockerhubuser/my_image:${env.BUILD_NUMBER}"
          docker.withRegistry('https://registry.hub.docker.com', 'docker_hub_creds_id') {
            def app = docker.image(dockerImage)
            app.push() // Push with the build number as tag
            app.push('latest') // Push with the 'latest' tag
          }
        }

      }
    }

  }
  environment {
    test_name = 'test_value'
  }
}