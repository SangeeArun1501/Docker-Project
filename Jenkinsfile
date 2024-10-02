pipeline {
    agent any
    environment {
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
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub') {
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
