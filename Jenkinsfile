pipeline {
    agent any

    environment {
        SONARQUBE_SERVER = 'http://34.48.73.26:9000'
        SONARQUBE_TOKEN = credentials('sonarqube')
        PATH = "${PATH}:/opt/sonar-scanner/bin"
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

        stage('Check SonarQube Connectivity') {
            steps {
                echo "Checking SonarQube connectivity..."
                sh 'curl -I ${SONARQUBE_SERVER}'
            }
        }

        stage('Verify SonarQube Scanner') {
            steps {
                echo "Verifying SonarQube scanner installation..."
                sh 'sonar-scanner -v'
            }
        }

        stage('Scan and Build Image') {
            steps {
                script {
                    echo "Starting SonarQube scan..."
                    withCredentials([string(credentialsId: 'sonarqube', variable: 'SONARQUBE_TOKEN')]) {
                        sh """
                            sonar-scanner \
                            -Dsonar.projectKey=jenkins_integration \
                            -Dsonar.sources=. \
                            -Dsonar.host.url=${SONARQUBE_SERVER} \
                            -Dsonar.login=\$SONARQUBE_TOKEN \
                            -Dsonar.java.binaries=target/classes
                        """
                    }
                    echo "Building Docker image..."
                    sh 'docker build -t ${DOCKER_USERNAME}/flaskapp .'
                    echo "Finished build."
                }
            }
        }
    }
}
