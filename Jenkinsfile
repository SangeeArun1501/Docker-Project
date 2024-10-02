pipeline {
    environment {
        docker_cred = credentials('dockerhub')
    }
    agent any
    stages {
        stage('Checkout Source') {
            steps {
                git 'https://github.com/SangeeArun1501/Docker-Project.git'
            }
        }
        stage('Build Images') {
            steps {
                script {
                    sh 'docker build -t $(dockerhub.username)/flaskapp .'
                    dir('mysql') {
                      sh 'docker build -t $(dockerhub.username)/mysql .'
                    }
                }
            }
        }
    }
}
   
