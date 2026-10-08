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

        stage('Deploy with k8s') {
            steps {
                script {
                    // Export variables for docker-compose
                    sh """
                        kubectl apply -f k8s/namespace.yaml
                        kubectl apply -f k8s/database/
                        kubectl apply -f k8s/backend/
                        kubectl apply -f k8s/frontend/
                        kubectl apply -f k8s/nginx/
                    """
                }
            }
        
        }
        stage('public access in browser') {
            steps{
                script{
                    sh """ kubectl port-forward svc/nginx -n my-app 8080:80 --address=0.0.0.0 & """
                }
            }
        }
        
    }
}
