pipeline {
  agent any

  environment {
    IMAGE_NAME = "lakshmanan1996/netflix-clone:latest"
  }

  stages {

    stage('Checkout Code') {
      steps {
        git 'https://github.com/Lakshmanan1996/Netflix-Clone.git'
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

    stage('Build Docker Image') {
      steps {
        sh 'docker build -t $IMAGE_NAME .'
      }
    }

    stage('Push Image to DockerHub') {
      steps {
        withCredentials([usernamePassword(
          credentialsId: 'dockerhub-creds',
          usernameVariable: 'DOCKER_USER',
          passwordVariable: 'DOCKER_PASS'
        )]) {
          sh 'docker login -u $DOCKER_USER -p $DOCKER_PASS'
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
