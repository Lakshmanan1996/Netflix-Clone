pipeline {
    agent any

    stages {

        stage('Checkout Source') {
            steps {
                echo "Checking out code from GitHub..."
                checkout scm
            }
        }

        stage('Debug Workspace') {
            steps {
                sh '''
                  echo "Workspace:"
                  pwd
                  echo "Files:"
                  ls -R
                '''
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                echo "Deploying to Minikube Kubernetes..."
                sh '''
                  kubectl apply -f k8s/
                  kubectl get pods
                  kubectl get svc
                '''
            }
        }
    }

    post {
        success {
            echo "✅ Netflix app deployed successfully to Kubernetes!"
        }
        failure {
            echo "❌ Deployment failed. Check logs."
        }
    }
}
