pipeline {
  agent any
    stages {
    stage('Checkout Source') {
      steps {
        git 'https://github.com/SangeeArun1501/Docker-Project.git'
      }
    }
      stage('Build code') {
        steps {
          def customImage = docker.build("flaskapp:latest")
        }
      }
  }

}
