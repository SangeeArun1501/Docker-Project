pipeline {
    agent any
    environment {
        DOCKER_CRED = credentials('dockerhub')
        FLASK_IMAGE = 'sangeetha1501/flaskapp'
        MYSQL_IMAGE = 'sangeetha1501/mysql'
    }
    stages {
        stage('Checkout Source') {
            steps {
                git 'https://github.com/SangeeArun1501/Docker-Project.git'
            }
        }
        stage('Build Images') {
            steps {
                script {
                    // Build the images
                    docker.build(FLASK_IMAGE)
                    dir('mysql') {
                        docker.build(MYSQL_IMAGE)
                    }
                }
            }
        }
        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', DOCKER_CRED) {
                        // Push the images
                        docker.image(FLASK_IMAGE).push()
                        docker.image(MYSQL_IMAGE).push()
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
