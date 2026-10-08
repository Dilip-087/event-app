pipeline {
    agent any
    
    environment {
        IMAGE_NAME = 'dilip087/event-app'
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
                    sh "docker build -t ${IMAGE_NAME}:v1 ."
                }
            }
        }
        
        stage('Push to Docker Hub') {
            steps {
                withCredentials([string(credentialsId: 'docker-hub-password', variable: 'DOCKER_PASSWORD')]) {
                    sh "echo ${DOCKER_PASSWORD} | docker login -u dilip087 --password-stdin"
                    sh "docker push ${IMAGE_NAME}:v1"
                }
            }
        }
        
        stage('Deploy to Kubernetes') {
            steps {
                sh "kubectl apply -f deployment.yaml"
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
