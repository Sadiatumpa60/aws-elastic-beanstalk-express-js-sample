pipeline {
  agent any   

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Install') {
      steps {
        sh 'echo "Node version check skipped (no Docker agent)"'
        sh 'echo "NPM install skipped (no Docker agent)"'
      }
    }

    stage('Test') {
      steps {
        sh 'echo "No tests run; this is a pipeline structure check."'
      }
    }
  }
}

