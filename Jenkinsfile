pipeline {
    agent any
    stages {
        stage('Checkout Source') {
            steps {
                git 'https://github.com/SangeeArun1501/Docker-Project.git'
            }
        }

        stage('Verify Sonar Scanner') {
            steps {
                sh 'sonar-scanner --version'  // Verify if the sonar-scanner is available
            }
        }
    }
}
