pipeline {
    agent any
    environment {
        DOCKER_CRED = credentials('dockerhub') // Ensure this matches your credential ID
        DOCKER_IMAGE_FLASK = "${DOCKER_CRED.USERNAME}/flaskapp" // Use username from credentials
        DOCKER_IMAGE_MYSQL = "${DOCKER_CRED.USERNAME}/mysql" // Use username from credentials
    }
    stages {
        stage('Checkout Source') {
            steps {
                git 'https://github.com/SangeeArun1501/Docker-Project.git'
            }
        }
        stage('Build code') {
            steps {
                script {
                    // Build the Flask app Docker image
                    def flaskImage = docker.build(DOCKER_IMAGE_FLASK)

                    // Build the MySQL Docker image
                    dir('mysql') {
                        def mysqlImage = docker.build(DOCKER_IMAGE_MYSQL)
                    }
                    echo 'Image Build Complete'

                    // Make the images available for the next stage
                    env.FLASK_IMAGE = flaskImage
                    env.MYSQL_IMAGE = mysqlImage
                }
            }
        }
        stage('Push to Docker Hub') {
            steps {
                script {
                    withDockerRegistry([ credentialsId: "dockerhub", url: "" ]) {
                        // Push the Flask image
                        docker.image(env.FLASK_IMAGE).push()
                        
                        // Push the MySQL image
                        docker.image(env.MYSQL_IMAGE).push()
                    }
                }
                echo 'Images pushed to Docker Hub'
            }
        }
    }
}


}
}
}
