pipeline {
    agent any

    environment {
        // Pull username + password from Jenkins credentials
        DOCKER_CREDS = credentials('dockerhub-creds')

        // Dynamic image names using DockerHub username
        FRONTEND_IMAGE = "${DOCKER_CREDS_USR}/frontend-app"
        BACKEND_IMAGE  = "${DOCKER_CREDS_USR}/backend-app"

        IMAGE_TAG = "${BUILD_NUMBER}"
    }

    stages {

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Images') {
            steps {
                script {
                    docker.build("${FRONTEND_IMAGE}:${IMAGE_TAG}", "./frontend")
                    docker.build("${BACKEND_IMAGE}:${IMAGE_TAG}", "./backend")
                }
            }
        }

        stage('Docker Login') {
            steps {
                sh """
                    echo $DOCKER_CREDS_PSW | docker login -u $DOCKER_CREDS_USR --password-stdin
                """
            }
        }

        stage('Push Images') {
            steps {
                sh """
                    docker push ${FRONTEND_IMAGE}:${IMAGE_TAG}
                    docker push ${BACKEND_IMAGE}:${IMAGE_TAG}

                    docker tag ${FRONTEND_IMAGE}:${IMAGE_TAG} ${FRONTEND_IMAGE}:latest
                    docker tag ${BACKEND_IMAGE}:${IMAGE_TAG} ${BACKEND_IMAGE}:latest

                    docker push ${FRONTEND_IMAGE}:latest
                    docker push ${BACKEND_IMAGE}:latest
                """
            }
        }

        stage('Deploy to VM') {
            steps {
                sh "docker-compose up -d"
                }
        }
        
    }

    post {
        success {
            echo "✅ Deployment Successful - Build ${BUILD_NUMBER}"
        }
        failure {
            echo "❌ Deployment Failed - Build ${BUILD_NUMBER}"
        }
        always {
            sh "docker logout"
        }
    }
}
