groovy
Copy code
pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('DevopsProject') // Jenkins credentials ID
        FRONTEND_IMAGE = 'sandarenuDT/frontend' // Your Docker Hub frontend image name
        BACKEND_IMAGE = 'sandarenuDT/backend'   // Your Docker Hub backend image name
        FRONTEND_TAG = 'latest'                 // Desired frontend image tag
        BACKEND_TAG = 'latest'                  // Desired backend image tag
    }
    stages {
        stage('SCM Checkout') {
            steps {
                git url: 'https://github.com/sandarenuDT/DevopsProject.git', branch: 'main' // Your GitHub repo URL and branch
            }
        }
        stage('Build Frontend Docker Image') {
            steps {
                script {
                    // Adjust paths if necessary
                    bat 'docker build -t %FRONTEND_IMAGE%:%FRONTEND_TAG% -f frontend\\Dockerfile frontend'
                }
            }
        }
        stage('Build Backend Docker Image') {
            steps {
                script {
                    // Adjust paths if necessary
                    bat 'docker build -t %BACKEND_IMAGE%:%BACKEND_TAG% -f backend\\Dockerfile backend'
                }
            }
        }
        stage('Login to Docker Hub') {
            steps {
                script {
                    withCredentials([string(credentialsId: 'DevopsProject', variable: 'DevopsProject')]) {
                        bat 'echo %DOCKERHUB_PASSWORD% | docker login -u sandarenudt --password-stdin'
                    }
                }
            }
        }
        stage('Push Frontend Image') {
            steps {
                script {
                    bat 'docker push %FRONTEND_IMAGE%:%FRONTEND_TAG%'
                }
            }
        }
        stage('Push Backend Image') {
            steps {
                script {
                    bat 'docker push %BACKEND_IMAGE%:%BACKEND_TAG%'
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