pipeline {
  agent any
  environment {
    IMAGE_NAME = "lakshmanan1996/netflix-clone:latest"
  }

  stages {
    stage('Checkout') {
      steps {
        git 'https://github.com/Lakshmanan1996/Netflix-Clone.git'
      }
    }

    stage('Build') {
      steps {
        sh 'yarn install'
        sh 'yarn build'
      }
    }

    stage('Docker Build & Push') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'dockerhub-creds',
        usernameVariable: 'USER', passwordVariable: 'PASS')]) {
          sh 'docker login -u $USER -p $PASS'
          sh 'docker build -t $IMAGE_NAME .'
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
