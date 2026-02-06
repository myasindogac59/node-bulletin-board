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
        stage('Docker  Deploy') {
            steps {
                sh '''
                echo "Eski Container Siliniyor "
                docker rm -f bulletin-board || true
                echo "Yeni Container Ayağa KAldırılıyor"
                docker run -d \
                -p 3000:8080 \
                --name bulletin-board \
                bulletin-board:${BUILD_NUMBER}
                '''
            }
        }
    }
}