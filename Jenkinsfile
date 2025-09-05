pipeline {
  agent any

  environment {
    # optional: Docker registry (if you push to DockerHub/registry)
    REGISTRY = ""
    IMAGE_TAG = "${env.BUILD_NUMBER}"
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Build & Compose Deploy') {
      steps {
        // make sure compose is available as `docker compose`
        sh '''
          # print versions for debug
          docker --version || true
          docker compose version || true

          # create .env from Jenkins credentials if you stored secrets in Jenkins (see notes)
          # Example: if you have a Jenkins secret text credential id 'BACKEND_ENV' that contains
          # KEY=val newline pairs, you can write it to backend/.env via pipeline credentials binding.

          # Build & (re)create services using local Docker engine
          docker compose -f docker-compose.yml pull || true
          docker compose -f docker-compose.yml up -d --build
        '''
      }
    }
  }

  post {
    success {
      echo "Deployment succeeded"
    }
    failure {
      echo "Deployment failed. Check logs."
    }
  }
}
