pipeline {
    agent any
    
    environment {
        registry = "isalmaniac/nodejsapp"
        registryCredential = 'dockerhub'
    }
    stages {
        
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
                  docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
                    docker.image.push("$BUILD_NUMBER")
                    docker.image.push('latest')
                  }
                }
            }
        }
    
        stage('Remove Unused docker image') {
          steps{
            sh "docker rmi $registry:$BUILD_NUMBER"
          }
        }
    }
}
