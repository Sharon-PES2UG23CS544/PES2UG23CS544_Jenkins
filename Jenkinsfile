pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Backend Image') {
            steps {
                // Remove old image if exists
                sh 'docker rmi -f backend-app || true'

                // Build Docker image using the backend folder
                sh 'docker build -t backend-app -f Dockerfile backend'
            }
        }

        stage('Deploy Backend Containers') {
            steps {
                sh 'docker run -d --name backend-container -p 8080:8080 backend-app'
            }
        }

        stage('Build NGINX Image') {
            steps {
                sh 'docker build -t nginx-lb nginx'
            }
        }

        stage('Deploy NGINX Load Balancer') {
            steps {
                sh 'docker run -d --name nginx-container -p 80:80 nginx-lb'
            }
        }
    }
}
