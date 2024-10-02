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
          sh 'docker build -t ${DOCKER_IMAGE_FLASK} .'
          dir('mysql') {
            sh 'docker build -t ${DOCKER_IMAGE_MYSQL} .'
          }
          echo ' Image Build Complete'
        }
      }
      stage('Push to Dockerhub') {
        steps {
          script {
          withDockerRegistry([ credentialsId: "dockerhub", url: "" ]) {
            dockerImage.push()
          }
        }
  }

}
}
}
