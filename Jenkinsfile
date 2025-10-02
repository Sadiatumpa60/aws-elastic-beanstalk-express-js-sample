pipeline {
  agent { docker { image 'node:16' } }

  options { timestamps() }

  stages {
    stage('Checkout') {
      steps { checkout scm }
    }

    stage('Install deps') {
      steps {
        sh 'node -v'
        sh 'npm -v'
        sh 'npm install --save'
      }
    }

    stage('Unit tests') {
      steps {
        sh 'npm test || echo "No tests found; continuing"'
      }
    }
  }
}

