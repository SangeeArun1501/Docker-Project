pipeline {
    agent any

    stages {
        stage('Check Docker Access') {
            steps {
                script {
                    echo "Checking Docker access..."
                    docker.image('alpine').inside {
                        sh 'echo "Docker is working!"'
                    }
                }
            }
        }
    }
}
