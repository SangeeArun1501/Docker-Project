pipeline {
    agent any
    environment {
        DOCKER_CRED = credentials('dockerhub')
        DOCKER_IMAGE_FLASK = 'sangeetha1501/flaskapp'
        DOCKER_IMAGE_MYSQL = 'sangeetha1501/mysql'
    }
    stages {
        stage('Checkout Source') {
            steps {
                git 'https://github.com/SangeeArun1501/Docker-Project.git'
            }
        }
        stage('Build code') {
            steps {
                script {
                    // Declare the image variables outside for later use
                    def flaskImage
                    def mysqlImage

                    // Build the images
                    flaskImage = docker.build(DOCKER_IMAGE_FLASK)
                    dir('mysql') {
                        mysqlImage = docker.build(DOCKER_IMAGE_MYSQL)
                    }
                    echo 'Image Build Complete'
                }
            }
        }
        stage('Push to Docker Hub') {
            steps {
                script {
                    echo "Pushing images without Docker Registry"
                    flaskImage.push()
                    mysqlImage.push()
                    }
                }
            }
        }
    }

