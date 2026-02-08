pipeline {
    agent any
    environment {
        APP_ENV = "development"
        APP_PORT = "3000"
    }
    stages {
        stage('Check Env') {
            steps {
                echo "Environment: ${APP_ENV}"
                echo "Port: ${APP_PORT}"
            }
        }
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