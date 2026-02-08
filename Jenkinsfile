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
                cd bulletin-boards-app
                npm install
                '''
            }
        }
    }
}