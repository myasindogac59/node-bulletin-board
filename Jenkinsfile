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
                script {
                    IMAGE_TAG = "env.BUILD_NUMBER"
                }
                sh '''
                cd bulletin-board-app
                docker build -t yasindogac5959/bulletin-board:${IMAGE_TAG} .
                docker tag yasindogac5959/bulletin-board:${IMAGE_TAG}  yasindogac5959/bulletin-board:latest
                '''
            }
        }
        stage('Push to Docker Hub') {
            steps {
                withCredentials([string(credentialsId: 'DOCKER_PASS', variable: 'DOCKER_PASS')]) {
                    sh '''
                    echo \$DOCKER_PASS | docker login -u yasindogac5959 --password-stdin
                    docker push yasindogac5959/bulletin-board:${IMAGE_TAG}
                    docker push yasindogac5959/bulletin-board:latest
                    docker logout
                    '''
                }
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                cd bulletin-board-app
                export BUILD_NUMBER=${env.BUILD_NUMBER}
                docker compose down  || true
                docker compose up -d
                '''
            }
        }
    }
}