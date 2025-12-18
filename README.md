# 🎬 Netflix Clone – Complete CI/CD DevOps Project

A **Netflix Clone application** deployed using a **production-grade CI/CD pipeline** demonstrating real-world DevOps practices including Jenkins automation, Docker, SonarQube, Trivy security scanning, Kubernetes orchestration, and AWS cloud deployment.

---

## 🚀 Project Highlights

* Automated CI/CD pipeline using **Jenkins**
* **Dockerized** Netflix Clone application
* **SonarQube** for code quality & static analysis
* **Trivy** for container security scanning
* **Kubernetes** deployment with rolling updates
* **AWS EC2 / EKS** cloud deployment
* Zero-downtime production-ready workflow

---

## 🏗️ CI/CD Architecture

```
Developer → GitHub → Jenkins → SonarQube → Docker → Trivy → DockerHub → Kubernetes → Live App
```

---

## 🔧 Tech Stack

| Category         | Tools         |
| ---------------- | ------------- |
| Version Control  | GitHub        |
| CI/CD            | Jenkins       |
| Code Quality     | SonarQube     |
| Containerization | Docker        |
| Security         | Trivy         |
| Orchestration    | Kubernetes    |
| Cloud            | AWS EC2 / EKS |
| Web Server       | Nginx         |

---

## ⚙️ Jenkinsfile (Complete)

```groovy
pipeline {
  agent any

  environment {
    IMAGE_NAME = "dockerhub_username/netflix:latest"
    DOCKERHUB_CREDENTIALS = 'dockerhub-creds'
  }

  stages {
    stage('Checkout Code') {
      steps {
        git 'https://github.com/username/netflix-clone.git'
      }
    }

    stage('SonarQube Analysis') {
      steps {
        sh 'sonar-scanner'
      }
    }

    stage('Build Docker Image') {
      steps {
        sh 'docker build -t $IMAGE_NAME .'
      }
    }

    stage('Trivy Scan') {
      steps {
        sh 'trivy image $IMAGE_NAME'
      }
    }

    stage('Push to DockerHub') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
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
```

---

## ☸️ Kubernetes YAML Files

### Deployment (`deployment.yaml`)

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: netflix-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: netflix
  template:
    metadata:
      labels:
        app: netflix
    spec:
      containers:
      - name: netflix
        image: dockerhub_username/netflix:latest
        ports:
        - containerPort: 80
```

### Service (`service.yaml`)

```yaml
apiVersion: v1
kind: Service
metadata:
  name: netflix-service
spec:
  type: NodePort
  selector:
    app: netflix
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30007
```

---

## ☁️ AWS Deployment (EC2 / EKS)

### EC2 (Minikube)

1. Launch EC2 (Ubuntu)
2. Install Docker, Jenkins, kubectl, Minikube
3. Start Minikube cluster
4. Run Jenkins pipeline
5. Access app via EC2 public IP

### EKS (Production)

1. Create EKS cluster using eksctl
2. Configure kubeconfig
3. Jenkins deploys directly to EKS
4. Expose app via LoadBalancer / Ingress

---

## 🎯 Interview Questions & Answers

### Q1. What is the purpose of CI/CD?

**CI/CD automates build, testing, and deployment to deliver software faster and reliably.**

### Q2. Why Docker is used?

**Docker ensures application consistency across environments using containers.**

### Q3. Role of Jenkins in this project?

**Jenkins automates the entire pipeline from code checkout to Kubernetes deployment.**

### Q4. Why SonarQube?

**To maintain code quality and detect bugs and vulnerabilities early.**

### Q5. What is Kubernetes rolling update?

**It updates pods gradually to ensure zero downtime.**

---

## 🧠 One-Line Resume Explanation

> Implemented a Netflix Clone using a full CI/CD pipeline with Jenkins, Docker, SonarQube, Trivy, and Kubernetes deployed on AWS with zero-downtime rolling updates.

---

## ⭐ Author

**Lakshmanan R**
DevOps Engineer | CI/CD | Kubernetes | AWS

---

## 📌 Future Enhancements

* HTTPS using Ingress & TLS
* Helm charts
* ArgoCD GitOps
* Monitoring with Prometheus & Grafana
