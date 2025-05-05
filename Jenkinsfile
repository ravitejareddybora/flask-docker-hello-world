pipeline {
  agent any

  environment {
    IMAGE_NAME = 'ravitejareddybora/docker-flask-jenk'
  }

  stages {
    stage('Checkout') {
      steps {
        // Jenkins already checked out 'production' for you
        echo "Building branch: ${env.GIT_BRANCH}"
      }
    }

    stage('Build') {
      steps {
        script {
          // tag with short commit SHA
          def sha = sh(script: 'git rev-parse --short HEAD', returnStdout: true).trim()
          docker.build("${IMAGE_NAME}:${sha}")
        }
      }
    }

    stage('Push') {
      steps {
        withDockerRegistry([credentialsId: 'dockerhub-creds', url: '']) {
          script {
            def sha = sh(script: 'git rev-parse --short HEAD', returnStdout: true).trim()
            docker.image("${IMAGE_NAME}:${sha}").push()
          }
        }
      }
    }
  }
}
