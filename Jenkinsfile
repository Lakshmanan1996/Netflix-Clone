pipeline {
    agent any

    environment {
        // Docker
        DOCKER_USER     = "lakshmanan1996"
        IMAGE_NAME      = "netflix"
        IMAGE_TAG       = "latest"

        // Kubernetes YAML files in k8s folder
        KUBE_DEPLOYMENT = "k8s/deployment.yaml"
        KUBE_SERVICE    = "k8s/service.yaml"
    }

    stages {

        stage('Checkout Source') {
            steps {
                echo "Checking out code from GitHub..."
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Building Docker image: $DOCKER_USER/$IMAGE_NAME:$IMAGE_TAG"
                sh '''
                  docker build -t $DOCKER_USER/$IMAGE_NAME:$IMAGE_TAG .
                '''
            }
        }

        stage('Docker Login') {
            steps {
                echo "Logging into Docker Hub..."
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                      echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                    '''
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                echo "Pushing Docker image to Docker Hub..."
                sh '''
                  docker push $DOCKER_USER/$IMAGE_NAME:$IMAGE_TAG
                '''
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                echo "Deploying to Kubernetes cluster..."
                withCredentials([file(credentialsId: 'kube-config-id', variable: 'KUBECONFIG_FILE')]) {
                    sh '''
                      export KUBECONFIG=$KUBECONFIG_FILE
                      kubectl apply -f $KUBE_DEPLOYMENT
                      kubectl apply -f $KUBE_SERVICE
                      echo "Verifying pods and services..."
                      kubectl get pods -o wide
                      kubectl get svc
                    '''
                }
            }
        }
    }

    post {
        success {
            echo "✅ CI/CD pipeline completed successfully!"
        }
        failure {
            echo "❌ Pipeline failed. Check console output for details."
        }
    }
}
