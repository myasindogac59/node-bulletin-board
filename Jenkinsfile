pipeline {
    agent any
    stages {
        stage('Test Workspace') {
            steps {
                sh '''
                pwd
                ls -la
                '''
            }
        }
        stage('Docker Build') {
            steps {
                sh '''
                docker build -t bulletin-board:${BUILD_NUMBER} bulletin-board-app
                '''
            }
        }
    }
}