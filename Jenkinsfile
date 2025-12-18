pipeline {
  agent any

  environment {
    IMAGE_NAME = "lakshmanan1996/netflix-clone:latest"
  }

  stages {

    stage('Checkout') {
      steps {
        echo "Code already checked out by Jenkins"
      }
    }

    stage('Install Dependencies') {
      steps {
        sh 'yarn install'
      }
    }

    stage('Build Application') {
      steps {
        sh 'yarn build'
      }
    }

    stage('Docker Build') {
      steps {
        sh 'docker build -t $IMAGE_NAME .'
      }
    }

    stage('Push to DockerHub') {
      steps {
        withCredentials([usernamePassword(
          credentialsId: 'dockerhub-creds',
          usernameVariable: 'USER',
          passwordVariable: 'PASS'
        )]) {
          sh 'docker login -u $USER -p $PASS'
          sh 'docker push $IMAGE_NAME'
        }
      }
    }

    stage('Deploy to Kubernetes') {
      steps {
        sh 'kubectl apply -f k8s/'
      }
    }
  }
}
