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

        stage('Deploy Backend Container') {
            steps {
                // Remove existing container if exists
                sh 'docker rm -f backend-container || true'

                // Run backend container
                sh 'docker run -d --name backend-container -p 8080:8080 backend-app'
            }
        }

        stage('Build NGINX Image') {
            steps {
                // Remove old image if exists
                sh 'docker rmi -f nginx-lb || true'

                // Build Docker image using nginx folder
                sh 'docker build -t nginx-lb nginx'
            }
        }

        stage('Deploy NGINX Load Balancer') {
            steps {
                // Remove existing container if exists
                sh 'docker rm -f nginx-container || true'

                // Run NGINX container
                sh 'docker run -d --name nginx-container -p 80:80 nginx-lb'
            }
        }
    }

    post {
        success {
            echo "Pipeline executed successfully!"
        }
        failure {
            echo "Pipeline failed! Check logs for errors."
        }
    }
}
