pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('DevopsProject') // Jenkins credentials ID
        FRONTEND_IMAGE = 'sandarenuDT/frontend'
        BACKEND_IMAGE = 'sandarenuDT/backend'
        FRONTEND_TAG = 'latest' // Use appropriate tags if needed
        BACKEND_TAG = 'latest' // Use appropriate tags if needed
    }
    stages {
        stage('SCM Checkout') {
            steps {
                git url: 'https://github.com/sandarenuDT/DevopsProject.git', branch: 'main'
            }
        }
        stage('Build Frontend Docker Image') {
            steps {
                script {
                    // Building frontend Docker image
                    bat 'docker build -t ${FRONTEND_IMAGE}:${FRONTEND_TAG} -f frontend/Dockerfile frontend'
                }
            }
        }
        stage('Build Backend Docker Image') {
            steps {
                script {
                    // Building backend Docker image
                    bat 'docker build -t ${BACKEND_IMAGE}:${BACKEND_TAG} -f backend/Dockerfile backend'
                }
            }
        }
        stage('Login to Docker Hub') {
            steps {
                script {
                    withCredentials([string(credentialsId: 'DevopsProject', variable: 'DOCKERHUB_PASSWORD')]) {
                        bat 'echo %DOCKERHUB_PASSWORD% | docker login -u sandarenuDT --password-stdin'
                    }
                }
            }
        }
        stage('Push Frontend Image') {
            steps {
                script {
                    bat 'docker push ${FRONTEND_IMAGE}:${FRONTEND_TAG}'
                }
            }
        }
        stage('Push Backend Image') {
            steps {
                script {
                    bat 'docker push ${BACKEND_IMAGE}:${BACKEND_TAG}'
                }
            }
        }
    }
    post {
        always {
            script {
                bat 'docker logout'
            }
        }
    }
}
