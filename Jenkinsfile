pipeline {
    agent any

    environment {
        DOCKER_USERNAME = 'sangeetha1501'
        IMAGE_NAME_FLASK = "${DOCKER_USERNAME}/flaskapp"
        IMAGE_NAME_MYSQL = "${DOCKER_USERNAME}/mysqlapp"
    }

    stages {
        stage('Checkout Code') {
            steps {
                echo "Checking out code from the Git repository"
                git branch: 'master', url: 'https://github.com/SangeeArun1501/Docker-Project.git'
            }
        }

        stage('Build Flask Docker Image') {
            steps {
                script {
                    echo "Building Flask Docker image..."
                    sh "docker build -t ${IMAGE_NAME_FLASK} ."
                    echo "Flask Docker image built successfully."
                }
            }
        }

        stage('Build MySQL Docker Image') {
            steps {
                script {
                    echo "Building MySQL Docker image..."
                    sh "docker build -t ${IMAGE_NAME_MYSQL} -f mysql/Dockerfile mysql/"
                    echo "MySQL Docker image built successfully."
                }
            }
        }
    }
}
