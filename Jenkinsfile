
pipeline {
    agent any 
    
    stages { 
        stage('SCM Checkout') {
            steps {
                retry(3) {
                    git branch: 'main', url: 'https://github.com/sandarenuDT/DevopsProject.git'
                }
            }
        }
        stage('Build Docker Image') {
            steps {  
                bat 'docker build -t sandarenudt/devopsproject:%BUILD_NUMBER% .'
            }
        }
        stage('Login to Docker Hub') {
            steps {
       
                 withCredentials([string(credentialsId: 'DevopsProject', variable: 'DevopsProject')]) {
                    script {
                        bat "docker login -u sandarenudt -p %DevopsProject%"
                    }
                }
            }
        }
        stage('Push Image') {
            steps {
                bat 'docker push sandarenudt/devopsproject:%BUILD_NUMBER%'
            }
        }
    }
    post {
        always {
            bat 'docker logout'
        }
    }
}
