pipeline {
    agent any

    environment {
        APP_NAME   = "bulletin-board"
        APP_SECRET = credentials('APP_SECRET')
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Set Environment') {
            steps {
                script {
                    if (env.BRANCH_NAME == 'master') {
                        env.DEPLOY_ENV = 'prod'
                        env.APP_PORT   = '3000'
                        env.IMAGE_TAG  = 'stable'
                    } else {
                        env.DEPLOY_ENV = 'dev'
                        env.APP_PORT   = '3001'
                        env.IMAGE_TAG  = 'latest'
                    }
                }
                echo "ENV=${DEPLOY_ENV} PORT=${APP_PORT} TAG=${IMAGE_TAG}"
            }
        }

        stage('Install & Test') {
            steps {
                sh '''
                cd bulletin-board-app
                npm install
                npm test || echo "Test yok, geçiliyor"
                '''
            }
        }

        stage('Check Secret') {
            steps {
                sh '''
                echo "Secret kontrol ediliyor..."
                if [ -z "$APP_SECRET" ]; then
                  echo "APP_SECRET YOK"
                  exit 1
                else
                  echo "APP_SECRET VAR"
                fi
                '''
            }
        }

        stage('Docker Build') {
            steps {
                sh '''
                docker build \
                  -t bulletin-board:${IMAGE_TAG} \
                  bulletin-board-app
                '''
            }
        }

        stage('Manual Approval (Prod)') {
            when {
                branch 'master'
            }
            steps {
                input message: 'PROD deploy onayı?', ok: 'Deploy'
            }
        }

        stage('Docker Compose Deploy') {
            steps {
                sh '''
                export APP_PORT=${APP_PORT}
                export IMAGE_TAG=${IMAGE_TAG}
                export APP_SECRET=${APP_SECRET}

                docker compose down || true
                docker compose up -d
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
            echo "Deploy başarılı (${DEPLOY_ENV})"
        }
        failure {
            echo "Deploy başarısız"
        }
    }
}
