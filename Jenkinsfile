pipeline {
    agent any
    stages {
        stage('Checkout Source') {
            steps {
                git 'https://github.com/SangeeArun1501/Docker-Project.git'
            }
        }
        stage('Build and Push Images') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    script {
                        sh "echo ${DOCKER_PASSWORD} | docker login -u ${DOCKER_USERNAME} --password-stdin"
                        echo ' build and push flask app'
                        sh "docker build -t ${DOCKER_USERNAME}/flaskapp ."
                        sh "docker push ${DOCKER_USERNAME}/flaskapp"
                        echo ' build and push mysql app'
                        dir('mysql') {
                            sh "docker build -t ${DOCKER_USERNAME}/mysql ."
                            sh "docker push ${DOCKER_USERNAME}/mysql"
                        }
                    }
                }
            }
        }
    }
}
