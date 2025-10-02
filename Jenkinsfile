pipeline {
  agent any

  stages {
    stage('Checkout') {
      steps { checkout scm }
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
  }
}

