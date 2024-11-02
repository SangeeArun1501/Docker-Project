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
                    // Enable debugging to see more details in logs
                    sh '''
                    set -x  # Enable debugging
                    docker build -t ${IMAGE_NAME_FLASK} .
                    '''
                    echo "Flask Docker image built successfully."
                }
            }
        }
    }
}
