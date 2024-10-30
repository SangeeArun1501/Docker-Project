pipeline {
    agent any

    environment {
        SONARQUBE_SERVER = 'http://34.48.73.26:9000'
        SONARQUBE_TOKEN = credentials('sonarqube')
        PATH = "${PATH}:/opt/sonar-scanner/bin"
        DOCKER_CREDENTIALS = 'dockerhub'
        DOCKER_USERNAME = 'sangeetha1501'
        KUBECONFIG = '/home/sakshara479/jenkins/kubeconfig'
    }

    stages {
        stage('Checkout Code') {
            steps {
                echo "Checkout code from the Git repository"
                git branch: 'master', url: 'https://github.com/SangeeArun1501/Docker-Project.git'
            }
        }
        stage('Scan and build image') {
            steps {
                script {
                    echo "Starting build..."
                    withCredentials([string(credentialsId: 'sonarqube', variable: 'SONARQUBE_TOKEN')]) {
                        sh """
                            sonar-scanner \
                            -Dsonar.projectKey=jenkins_integration \
                            -Dsonar.sources=. \
                            -Dsonar.host.url=${SONARQUBE_SERVER} \
                            -Dsonar.login=${SONARQUBE_TOKEN} \
                            -Dsonar.java.binaries=target/classes
                        """
                    }
                    sh 'docker build -t ${DOCKER_USERNAME}/flaskapp .'
                    echo "Finished build."
                }
            }
        }
    }
}
