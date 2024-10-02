pipeline {
    agent any
    stages {
        stage('Check Credentials') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    script {
                        echo "Docker Username: ${DOCKER_USERNAME}"
                    }
                }
            }
        }
    }
}
