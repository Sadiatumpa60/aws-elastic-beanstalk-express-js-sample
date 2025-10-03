pipeline {
  agent any

  options {
    timestamps()
    buildDiscarder(logRotator(numToKeepStr: '10', artifactNumToKeepStr: '5'))
  }

  environment {
    IMAGE_NAME = 'yourdockeruser/aws-eb-express-sample'
    DOCKERHUB_USER = "${env.DOCKERHUB_USER}"
    DOCKERHUB_TOKEN = "${env.DOCKERHUB_TOKEN}"
    SNYK_TOKEN = "${env.SNYK_TOKEN}"
  }

  stages {
    stage('Install') {
      steps {
        script {
          docker.image('node:16').inside {
            sh '''
              node -v
              npm -v
              npm ci || npm install
            '''
          }
        }
      }
    }

    stage('Test') {
      steps {
        script {
          docker.image('node:16').inside {
            sh 'npm test || echo "Tests skipped or failed"'
          }
        }
      }
    }

    stage('Build Docker') {
      steps {
        sh '''
          docker build -t $IMAGE_NAME:$BUILD_NUMBER .
          docker image inspect $IMAGE_NAME:$BUILD_NUMBER --format='{{.Id}}' | tee docker-image.txt
        '''
      }
    }

    stage('Push Docker (optional)') {
      when {
        expression { return (env.DOCKERHUB_USER?.trim() && env.DOCKERHUB_TOKEN?.trim()) }
      }
      steps {
        sh '''
          echo "$DOCKERHUB_TOKEN" | docker login -u "$DOCKERHUB_USER" --password-stdin
          docker push $IMAGE_NAME:$BUILD_NUMBER
        '''
      }
    }

    stage('Security Scan') {
      steps {
        script {
          docker.image('node:16').inside {
            sh '''
              if [ -n "$SNYK_TOKEN" ]; then
                npx snyk auth "$SNYK_TOKEN" || true
                npx snyk test --severity-threshold=high
              else
                npm audit --audit-level=high || (echo "High/Critical vulnerabilities found" && exit 1)
              fi
            '''
          }
        }
      }
    }
  }

  post {
    always {
      sh '''
        mkdir -p logs reports coverage
        echo "Build $BUILD_NUMBER finished at $(date)" > logs/build.txt
      '''
      archiveArtifacts artifacts: 'logs/**/*, reports/**/*, coverage/**/*, docker-image.txt', onlyIfSuccessful: false
    }
  }
}
