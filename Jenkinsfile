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
          sh 'docker build -t flaskapp .'
          dir('mysql') {
            sh 'docker build -t mysql .'
          }
        }
      }
  }

}
