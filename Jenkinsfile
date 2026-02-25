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
                sh 'docker rmi -f backend-app || true'
                sh 'docker build -t backend-app -f Dockerfile backend'
            }
        }

        stage('Deploy Backend Container') {
            steps {
                sh 'docker rm -f backend-container || true'
                sh 'docker run -d --name backend-container -p 8081:8080 backend-app'
            }
        }

        stage('Build NGINX Image') {
            steps {
                sh 'docker rmi -f nginx-lb || true'
                sh 'docker build -t nginx-lb nginx'
            }
        }

        stage('Deploy NGINX Load Balancer') {
            steps {
                sh 'docker rm -f nginx-container || true'
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
