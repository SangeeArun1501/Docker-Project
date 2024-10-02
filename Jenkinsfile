pipeline {
    agent any
    environment {
        DOCKER_CRED = credentials('dockerhub')  // Jenkins credential ID for Docker Hub
        DOCKER_IMAGE_FLASK = 'sangeetha1501/flaskapp'
        DOCKER_IMAGE_MYSQL = 'sangeetha1501/mysql'
    }
    stages {
        stage('Checkout Source') {
            steps {
                echo 'Checking out the source code...'
                git 'https://github.com/SangeeArun1501/Docker-Project.git'
            }
        }
        stage('Build code') {
            steps {
                script {
                    echo 'Building Flask image...'
                    def flaskImage = docker.build(DOCKER_IMAGE_FLASK)
                    
                    dir('mysql') {
                        echo 'Building MySQL image...'
                        def mysqlImage = docker.build(DOCKER_IMAGE_MYSQL)
                    }
                    
                    echo 'Image Build Complete'
                    echo "Flask Image: ${flaskImage}"
                    echo "MySQL Image: ${mysqlImage}"
                }
            }
        }
        stage('Push to Docker Hub') {
            steps {
                script {
                    echo 'Starting Docker push process...'
                    
                    // Authenticate and push the images
                    withDockerRegistry([ credentialsId: 'dockerhub', url: '' ]) {
                        try {
                            echo 'Pushing Flask image...'
                            flaskImage.push()
                            echo 'Flask image pushed successfully.'
                        } catch (Exception e) {
                            echo "Error pushing Flask image: ${e.getMessage()}"
                        }
                        
                        try {
                            echo 'Pushing MySQL image...'
                            mysqlImage.push()
                            echo 'MySQL image pushed successfully.'
                        } catch (Exception e) {
                            echo "Error pushing MySQL image: ${e.getMessage()}"
                        }
                    }
                }
            }
        }
    }
    post {
        always {
            echo 'Pipeline completed.'
        }
    }
}
