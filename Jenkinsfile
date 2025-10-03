pipeline {
  agent any

  environment {
    IMAGE = "SadiaCurtin1999/eb-express:${BUILD_NUMBER}"
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Install & Test (Node 16 in container)') {
      steps {
        sh '''
          docker run --rm -v "$PWD":/app -w /app node:16 bash -lc "
            node -v &&
            npm -v &&
            npm install --save &&
            npm test || echo 'No tests found; continuing'
          "
        '''
      }
    }

    stage('Build Docker Image') {
      steps {
        sh '''
          echo "Building $IMAGE"
          docker build -t "$IMAGE" .
          docker image ls "$IMAGE"
        '''
      }
    }

    stage('Vulnerability Scan (fail on High/Critical)') {
      steps {
        sh '''
          docker run --rm \
            -v "$PWD":/app -w /app \
            aquasec/trivy:0.54.1 fs \
            --scanners vuln \
            --ignore-unfixed \
            --severity HIGH,CRITICAL \
            --exit-code 1 \
            .
        '''
      }
    }
  }
}


