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
        stage('SonarQube Image Scan') {
            steps {
                script {
                    // Run SonarQube Scanner for Docker image
                    withCredentials([string(credentialsId: 'your_sonarqube_token_id', variable: 'SONAR_TOKEN')]) {
                        withSonarQubeEnv('SonarQube') {
                            sh """
                            docker run --rm \
                            -e SONAR_HOST_URL='http://<your-sonarqube-url>' \ // Replace with actual URL
                            -e SONAR_TOKEN='${SONAR_TOKEN}' \
                            -v \$(pwd):/src \
                            sonarsource/sonar-scanner-cli \
                            sonar-scanner \
                            -Dsonar.projectKey=jenkins_integration \
                            -Dsonar.sources=/src
                            """
                        }
                    }
                }
            }
        }
        stage('Push Images') {
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
