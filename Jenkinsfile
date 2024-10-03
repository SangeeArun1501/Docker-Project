pipeline {
    agent any
    stages {
        stage('Checkout Source') {
            steps {
                git 'https://github.com/SangeeArun1501/Docker-Project.git'
            }
        }
         stage('SonarQube Scan') {
            steps {
                script {
                    // Run SonarQube Scanner for source code
                    withSonarQubeEnv('SonarQube') {
                        sh 'sonar-scanner -Dsonar.projectKey=jenkins_integration -Dsonar.sources=.'
                    }
                }
            }
        }
        stage('Build Images') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    script {
                        sh "echo ${DOCKER_PASSWORD} | docker login -u ${DOCKER_USERNAME} --password-stdin"
                        echo 'building'
                        sh "docker build -t ${DOCKER_USERNAME}/flaskapp ."
                        dir('mysql') {
                            sh "docker build -t ${DOCKER_USERNAME}/mysql ."
                        }
                    }
                }
            }
        }
        stage('Build Images') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    script {
                        sh "echo ${DOCKER_PASSWORD} | docker login -u ${DOCKER_USERNAME} --password-stdin"
                        echo 'Pushing'
                        sh "docker push ${DOCKER_USERNAME}/flaskapp"
                        sh "docker push ${DOCKER_USERNAME}/mysql"
                        }
                    }
                }
            }
        }
    }
}
