pipeline {
    agent any
    environment {
            APP_ENV = credentials('APP_ENV')
            APP_PORT = credentials('APP_PORT')
            IMAGE_NAME = "yasindogac5959/bulletin-board"
            IMAGE_TAG = "${BUILD_NUMBER}"
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
                docker build -t $IMAGE_NAME:$IMAGE_TAG .
                '''
            }
        }
        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                    echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                    docker push $IMAGE_NAME:$IMAGE_TAG
                    docker logout
                    '''
                }
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                cd bulletin-board-app
                docker rm -f bulletin-board-app || true
                docker compose down  || true
                docker compose up -d
                '''
            }
        }
    }
}