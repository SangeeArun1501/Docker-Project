pipeline {
  agent any
  environment {
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
      stage('Push to Dockerhub') {
        steps {
          script {
          withDockerRegistry([ credentialsId: "dockerhub", url: "" ]) {
            flaskImage.push()
            mysqlImage.push()
          }
        }
  }

}
}
}
