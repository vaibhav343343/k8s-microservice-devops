pipeline {
    agent any

    environment {
        IMAGE_NAME = "vaibhav343343/flask-microservice"
        IMAGE_TAG  = "latest"
    }

    stages {

        stage('Clone Repo') {
            steps {
                echo "Cloning GitHub repository"
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Building Docker image"
                sh '''
                docker build -t $IMAGE_NAME:$IMAGE_TAG .
                '''
            }
        }

        stage('Login to Docker Hub') {
            steps {
                echo "Logging in to Docker Hub"
                sh '''
                echo "$DOCKERHUB_PASSWORD" | docker login -u "$DOCKERHUB_USERNAME" --password-stdin
                '''
            }
        }

        stage('Push Docker Image') {
            steps {
                echo "Pushing image to Docker Hub"
                sh '''
                docker push $IMAGE_NAME:$IMAGE_TAG
                '''
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                echo "Deploying to Kubernetes"
                sh '''
                kubectl apply -f k8s/deployment.yaml
                kubectl apply -f k8s/service.yaml
                '''
            }
        }
    }

    post {
        success {
            echo "✅ CI/CD Pipeline completed successfully"
        }
        failure {
            echo "❌ Pipeline failed"
        }
    }
}

