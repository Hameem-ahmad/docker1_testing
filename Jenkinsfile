pipeline {
    agent any

    environment {
        IMAGE_NAME = 'reactapp'
        IMAGE_TAG  = "${env.BRANCH_NAME}-${env.BUILD_NUMBER}"
        CI         = 'true'
    }

    stages {
        stage('Install, Test & Build') {
            agent {
                docker {
                    image 'node:22-alpine'
                    args '-e npm_config_cache=/tmp/.npm'
                    reuseNode true
                }
            }
            steps {
                sh 'npm ci'
                sh 'npm test'
                sh 'npm run build'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ${IMAGE_NAME}:${IMAGE_TAG} .'
            }
        }

        stage('Smoke Test') {
            steps {
                sh '''
                  docker rm -f smoke || true
                  docker run -d --name smoke -p 8090:80 ${IMAGE_NAME}:${IMAGE_TAG}
                  sleep 5
                  curl -f http://localhost:8090
                '''
            }
            post {
                always { sh 'docker rm -f smoke || true' }
            }
        }
    }

    post {
        success { echo "Build OK: ${IMAGE_NAME}:${IMAGE_TAG}" }
        failure { echo 'Build FAILED' }
    }
}
