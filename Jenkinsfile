pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Install Dependencies') {
            steps {
                sh '''
                cd bulletin-board-app
                npm install
                '''
            }
        }
        stage('Build Docker Image') {
            steps {
                sh '''
                cd bulletin-board-app
                docker build -t bulletin-board-app:latest .
                '''
            }
        }
    }
}