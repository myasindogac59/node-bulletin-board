pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Workspace Kontrol') {
            steps {
                sh '''
                echo "Bulunduğum Dizin:"
                pwd
                echo "Dosyalar:"
                ls -la
                '''
            }
        }
    }
}