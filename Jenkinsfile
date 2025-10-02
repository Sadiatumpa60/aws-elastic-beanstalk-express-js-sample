  stage('Build Docker Image') {
    steps {
      sh '''
        IMAGE="<sadiacurtin1999>/eb-express:${BUILD_NUMBER}"
        echo "Building $IMAGE"
        docker build -t "$IMAGE" .
        docker image ls | head -n 10
      '''
    }
  }


