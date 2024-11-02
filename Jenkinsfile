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
                    // Check Docker access by running a simple command
                    sh 'docker info'
                }
            }
        }
    }
}

