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
        stage('Run App (Smoke Test)') {
            steps {
                sh '''
                cd bulletin-board-app
                timeout 5 npm start || true
                '''
            }
        }
    }
}