pipeline {
  agent any

  stages {
    stage('Clone') {
      steps {
        checkout scm
      }
    }

    stage('Build Docker Image') {
      steps {
        sh 'eval $(minikube docker-env) && docker build -t k8s-microservice:latest .'
      }
    }

    stage('Deploy to Kubernetes') {
      steps {
        sh 'kubectl apply -f k8s/deployment.yaml'
        sh 'kubectl apply -f k8s/service.yaml'
      }
    }
  }
}
