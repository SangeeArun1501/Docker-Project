pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                echo "Checking out code from the Git repository"
                git url: 'https://github.com/SangeeArun1501/Docker-Project.git'
            }
        }
        stage('Check Docker Access') {
            steps {
                script {
                    echo "Checking Docker access..."            
                    try {
                        sh 'docker info'
                        } catch (Exception e) {
                        echo "Docker access failed: ${e.message}"
                        error("Docker access check failed.")
                        }
                }
            }
        }
    }
}
