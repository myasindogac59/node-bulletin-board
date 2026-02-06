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
        stage('Docker Run') {
            steps {
                sh '''
                docker rm -f bulletin-board || true
                docker run -d \
                -p 3000:8080 \
                --name bulletin-board:7
                '''
            }
        }
    }
}