pipeline {
    agent any

    environment {
        DOCKER_CREDS = credentials('dockerhub-creds')
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
            parallel {
                stage('Frontend') {
                    steps {
                        script {
                            docker.build("${FRONTEND_IMAGE}:${IMAGE_TAG}", "./frontend")
                        }
                    }
                }
                stage('Backend') {
                    steps {
                        script {
                            docker.build("${BACKEND_IMAGE}:${IMAGE_TAG}", "./backend")
                        }
                    }
                }
            }
        }

        stage('Push Images') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-creds') {
                        docker.image("${FRONTEND_IMAGE}:${IMAGE_TAG}").push()
                        docker.image("${BACKEND_IMAGE}:${IMAGE_TAG}").push()
                        docker.image("${FRONTEND_IMAGE}:${IMAGE_TAG}").push('latest')
                        docker.image("${BACKEND_IMAGE}:${IMAGE_TAG}").push('latest')
                    }
                }
            }
        }

        stage('Deploy with Docker Compose') {
            steps {
                dir("${WORKSPACE}") {
                    // Export variables for docker-compose
                    sh """
                        export FRONTEND_IMAGE=${FRONTEND_IMAGE}
                        export BACKEND_IMAGE=${BACKEND_IMAGE}
                        export IMAGE_TAG=${IMAGE_TAG}
                        docker-compose down || true
                        docker-compose up -d
                    """
                }
            }
        }
    }
}
