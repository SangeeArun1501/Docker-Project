pipeline {
  agent any
  environment {
    DOCKER_CRED = credentials('dockerhub')
    DOCKER_IMAGE_FLASK = 'sangeetha1501/flaskapp'
    DOCKER_IMAGE_MYSQL = 'sangeetha1501/mysql'
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
           def flaskImage = docker.build(DOCKER_IMAGE_FLASK)
          dir('mysql') {
           def mysqlImage = docker.build(DOCKER_IMAGE_MYSQL)
                    }
          echo 'Image Build Complete'
                }
            }
      }
      stage('Push to Docker Hub') {
            steps {
                script {
                    // Use withDockerRegistry for pushing images
                    withDockerRegistry([ credentialsId: 'dockerhub', url: '' ]) {
                        // Push the Flask image
                        flaskImage.push()
                        // Push the MySQL image
                        mysqlImage.push()
                    }
                }
            }
        }
}
}
