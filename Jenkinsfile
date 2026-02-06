pipeline {
    agent any
    stages{
        stage('Checkout') {
            steps {
                checkout scm
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
                docker compose down || true
                docker compose up -d
                '''
            }
        }
    }
}