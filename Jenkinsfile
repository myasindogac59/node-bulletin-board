pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Node Control') {
            steps {
                sh '''
                echo "Node vversiyonu:"
                node -v || echo "Node Yok"
                
                echo "npm versiyonu:"
                npm -v || echo || "npm yok"
                '''
            }
        }
    }
}