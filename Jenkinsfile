pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                echo "Checking out code from the Git repository"
                git url: 'https://github.com/SangeeArun1501/Docker-Project.git'
            }
        }
    }

    post {
        success {
            echo "Checkout completed successfully!"
        }
        failure {
            echo "Checkout failed!"
        }
    }
}
