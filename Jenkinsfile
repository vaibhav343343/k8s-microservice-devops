pipeline {
    agent any

    environment {
        IMAGE_NAME = "vaibzz7219/flask-microservice"
        IMAGE_TAG  = "latest"
    }

    stages {

        stage('Clone Repo') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                docker build -t $IMAGE_NAME:$IMAGE_TAG .
                '''
            }
        }

        stage('Login & Push to Docker Hub') {
            steps {
                withCredentials([
                    string(credentialsId: 'DOCKERHUB_USERNAME', variable: 'DH_USER'),
                    string(credentialsId: 'DOCKERHUB_PASSWORD', variable: 'DH_PASS')
                ]) {
                    sh '''
                    echo "$DH_PASS" | docker login -u "$DH_USER" --password-stdin
                    docker push $IMAGE_NAME:$IMAGE_TAG
                    '''
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
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
            echo "❌ CI/CD Pipeline failed"
        }
    }
}
