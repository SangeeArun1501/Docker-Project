pipeline {
    agent any
    environment {
        docker_cred = credentials('dockerhub') // Your Docker Hub credentials
        docker_username = credentials('dockerhub-username') // New credential for username
        docker_image_flask = "${docker_username}/flaskapp" // Image name for Flask app
        docker_image_mysql = "${docker_username}/mysql" // Image name for MySQL
    }
    stages {
        stage('Checkout Source') {
            steps {
                git 'https://github.com/SangeeArun1501/Docker-Project.git'
            }
        }
        stage('Build code') {
            steps {
                // Build Flask app Docker image
                sh "docker build -t ${docker_image_flask} ."
                
                // Build MySQL Docker image
                dir('mysql') {
                    sh "docker build -t ${docker_image_mysql} ."
                }
                echo 'Image Build Complete'
            }
        }
        stage('Push to Docker Hub') {
            steps {
                // Login to Docker Hub using the credentials
                sh "echo ${docker_cred_PASSWORD} | docker login -u ${docker_cred_USERNAME} --password-stdin"
                echo 'Login successful'
                
                // Push the Flask app image to Docker Hub
                sh "docker push ${docker_image_flask}"
                
                // Push the MySQL image to Docker Hub
                sh "docker push ${docker_image_mysql}"
            }
        }
    }
}

