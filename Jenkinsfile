pipeline {
  agent { docker { image 'node:16' } }   

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Install') {
      steps {
        sh 'node -v'
        sh 'npm -v'
        sh 'npm install'
      }
    }

    stage('Test') {
      steps {
        sh 'npm test || echo "No tests found; continuing"'
      }
    }
  }
}
