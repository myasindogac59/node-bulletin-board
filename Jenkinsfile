pipeline {
    agent any
    stages{
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Install & Test') {
            steps {
                sh '''
                cd bulletin-board-app
                npm install
                npm test || echo "No test defined, skipping"
                '''
            }
        }
        stage('Docker Build') {
            steps {
                sh '''
                docker build \
                -t bulletin-board:${BUILD_NUMBER} \
                bulletin-board-app
                '''
            }
        }
        stage('Docker Compose Deploy') {
            steps {
                sh '''
                docker-compose down || true
                docker-compose up -d
                '''
            }
        }
        stage('Health Check') {
            steps {
                sh '''
                echo "Waiting for app to start..."
                sleep 5
                curl -f http://localhost:3000 || exit 1
                '''
            }
        }
    }
}