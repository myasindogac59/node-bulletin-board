pipeline {
    agent any
    environment {
            APP_ENV = credentials('APP_ENV')
            APP_PORT = credentials('APP_PORT')
        }
    stages {
        stage('Check Env') {
            steps {
                sh '''
                echo "Environment: $APP_ENV"
                echo "Port: $APP_PORT"
                '''
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
        stage('Run App (Smoke Test)') {
            steps {
                sh '''
                cd bulletin-board-app
                timeout 5 npm start || true
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

        stage('Deploy with Docker Compose') {
            steps {
                sh '''
                cd bulletin-board-app
                echo "Stopping old containers"
                docker-compose down || true
                echo "Starting new containers"
                docker compose up -d --build
                '''
            }
        }
    }
}