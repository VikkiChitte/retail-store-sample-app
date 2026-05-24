pipeline {
    agent any
    
    environment {
        awsCredentialsId = 'ecr:us-east-1:awscreds'
        awsEcrUrl = 'https://848360037717.dkr.ecr.us-east-1.amazonaws.com'
        imageName = '848360037717.dkr.ecr.us-east-1.amazonaws.com/cartimg'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build( "${imageName}:${BUILD_NUMBER}", "./src/cart/" )
                }
            }
        }

        stage('Upload Cart Image to ECR') {
            steps {
                script {
                    docker.withRegistry(awsEcrUrl, awsCredentialsId) {
                        dockerImage.push("$BUILD_NUMBER")
                        dockerImage.push("latest")
                    }
                }
            }
        }
        stage('Clean Up') {
            steps {
                script {
                    dockerImage.remove()
                }
            }
        }
    }
    post {
        success {
            echo "Image pushed to ECR successfully: ${imageName}:${BUILD_NUMBER}"
        }
        failure {
            echo "Failed to build or push image: ${imageName}:${BUILD_NUMBER}"
        }
    }
        
}