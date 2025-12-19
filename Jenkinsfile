pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "lakshmanan1996/netflix:latest"
        KUBE_DEPLOYMENT_FILE = "k8s/deployment.yaml"
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
                echo "Building Docker image: ${DOCKER_IMAGE}"
                sh """
                    docker build -t ${DOCKER_IMAGE} .
                """
            }
        }

        stage('Docker Login') {
            steps {
                echo "Logging into Docker Hub..."
                withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh """
                        echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                    """
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                echo "Pushing Docker image to Docker Hub..."
                sh """
                    docker push ${DOCKER_IMAGE}
                """
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                echo "Deploying to Kubernetes cluster..."
                withCredentials([file(credentialsId: 'kubeconfig', variable: 'KUBECONFIG_FILE')]) {
                    sh """
                        export KUBECONFIG=$KUBECONFIG_FILE
                        kubectl apply -f ${KUBE_DEPLOYMENT_FILE}
                        echo "Verifying pods and services..."
                        kubectl get pods -o wide
                        kubectl get svc
                    """
                }
            }
        }
    }

    post {
        success {
            echo "✅ Pipeline completed successfully!"
        }
        failure {
            echo "❌ Pipeline failed. Check console output for details."
        }
    }
}
