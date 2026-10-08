pipeline {
    agent any
    
    environment {
        IMAGE_NAME = 'dilip087/event-app'
        // Full paths for Windows system execution
        DOCKER_PATH = 'C:\\Program Files\\Docker\\Docker\\resources\\bin\\docker.exe'
        KUBECTL_PATH = 'C:\\Program Files\\Docker\\Docker\\resources\\bin\\kubectl.exe'
    }
    
    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }
        
        stage('Build Docker Image (v1)') {
            steps {
                script {
                    bat "\"${env.DOCKER_PATH}\" build -t ${IMAGE_NAME}:v1 ."
                }
            }
        }
        
        stage('Push to Docker Hub') {
            steps {
                withCredentials([string(credentialsId: 'docker-hub-password', variable: 'DOCKER_PASSWORD')]) {
                    bat "echo %DOCKER_PASSWORD% | \"${env.DOCKER_PATH}\" login -u dilip087 --password-stdin"
                    bat "\"${env.DOCKER_PATH}\" push ${IMAGE_NAME}:v1"
                }
            }
        }
        
        stage('Deploy to Kubernetes') {
            steps {
                bat "\"${env.KUBECTL_PATH}\" apply -f deployment.yaml"
            }
        }
    }
    
    post {
        success {
            echo 'Pipeline executed successfully! Application deployed with 3 replicas.'
        }
        failure {
            echo 'Pipeline failed during execution.'
        }
    }
}
