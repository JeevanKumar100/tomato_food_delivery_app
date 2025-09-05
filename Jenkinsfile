pipeline {
  agent any

  environment {
    // Optional: Docker registry (if you push to DockerHub/registry)
    REGISTRY = ""
    IMAGE_TAG = "${env.BUILD_NUMBER}"
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Build & Deploy with Docker Compose') {
      steps {
        sh '''
          echo "=== Docker versions ==="
          docker --version || true
          docker compose version || true

          # Rebuild and run services
          docker compose -f docker-compose.yml up -d --build
        '''
      }
    }
  }

  post {
    success {
      echo "✅ Deployment succeeded"
    }
    failure {
      echo "❌ Deployment failed. Check logs."
    }
  }
}
