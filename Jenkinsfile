pipeline {
    agent any
    stages {
        stage('Checkout Source') {
            steps {
                git 'https://github.com/SangeeArun1501/Docker-Project.git'
            }
        }
        stage('Build Images') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    script {
                        sh "docker build -t ${DOCKER_USERNAME}/flaskapp ."
                        dir('mysql') {
                            sh "docker build -t ${DOCKER_USERNAME}/mysql ."
                        }
                    }
                }
            }
        }
    }
}
