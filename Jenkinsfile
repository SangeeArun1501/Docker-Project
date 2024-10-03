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
                script {
                    def sonarScannerPath = '/opt/sonar-scanner/bin/sonar-scanner' // Change this to your scanner path
                    sh "${sonarScannerPath} --version"  // Verify if the sonar-scanner is available
            }
        }
    }
}
}
