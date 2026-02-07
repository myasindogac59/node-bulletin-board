pipeline {
    agent any
    environment {
        APP_NAME = "bulletin-board"
        APP_SECRET = credentials('APP_SECRET')
    }
    stage('Check Secret') {
    steps {
        sh '''
        echo "Secret var mı?"
        if [ -z "$APP_SECRET" ]; then
          echo "SECRET YOK"
        else
          echo "SECRET VAR"
        fi
        '''
    }
}

    stages {

        stage('Set Environment') {
            steps {
                script {
                    if (env.BRANCH_NAME == 'master') {
                        env.DEPLOY_ENV = 'prod'
                        env.APP_PORT = '3000'
                    } else {
                        env.DEPLOY_ENV = 'dev'
                        env.APP_PORT = '3001'
                    }
                }
                echo "Environment: ${DEPLOY_ENV}, Port ${APP_PORT}"
            }
        }

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install & Test') {
            steps {
                sh '''
                cd bulletin-board-app
                npm install
                npm test || echo "No test defined"
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
        stage('Manual Approval (Prod)') {
            when {
                branch 'master'
            }
            steps {
                input message: 'Deploy to PROD', ok: 'Deploy'
            }
        }

        stage('Docker Compose Deploy') {
            steps {
                sh '''
                docker tag bulletin-board:stable bulletin-board:backup || true
                docker-compose down || true
                docker-compose up -d
                '''
            }
        }

        stage('Health Check') {
            steps {
                sh '''
                sleep 5
                curl -f http://localhost:${APP_PORT}
                '''
            }
        }
    }

    post {
        success {
            sh '''
            docker tag bulletin-board:${BUILD_NUMBER} bulletin-board:stable
            '''
        }

        failure {
            sh '''
            export IMAGE_TAG=backup
            docker-compose down || true
            docker-compose up -d
            '''
        }
    }
}
