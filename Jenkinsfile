pipeline {
    agent any
    environment {
        APP_ENV = "development"
        APP_PORT = "4000"
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
        environment {
            APP_ENV = credentials('APP_ENV')
            APP_PORT = credentials('APP_PORT')
        }
        stage('Run Docker Container') {
            steps {
                sh '''
                echo "Stopping and removing any existing container..."
                docker rm -f bulletin-board || true
                echo "Running new container..."
                docker run  -d \
                    -p ${APP_PORT}:${APP_PORT} \
                    -e APP_ENV= ${APP_ENV} \
                    -e APP_PORT= ${APP_PORT} \
                    bulletin-board-app:latest
                    '''
            }
        }
    }
}