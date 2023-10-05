pipeline {
    agent any
    environment {
        registry = "isalmaniac/nodejsapp"
        registryCredential = 'dockerhub'
    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from your source code repository (e.g., Git)
                checkout scm
            }
        }
        
        stage('Build Docker Image') {
            steps {
                // Build the Docker image
                script {
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                }
            }
        }
        
        stage('Push to Docker Hub') {
            steps {
                // Push the Docker image to Docker Hub
                script {
                  docker.withRegistry( '', registryCredential ) {
                    docker.image.push("$BUILD_NUMBER")
                    docker.image.push('latest')
                  
                }
            }
        }
    
        stage('Remove Unused docker image') {
          steps{
            sh "docker rmi $registry:$BUILD_NUMBER"
          }
        }
    
    post {
        success {
            // Deploy the application to the Nginx server here
            // You can use SSH or other deployment methods depending on your server setup
            echo 'Successfully Deployed'
        }
    }
}

