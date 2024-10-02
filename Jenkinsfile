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
          sh 'docker run -p 8080:8080 -d flaskapp'
          sh 'docker run -p 3306:3306 -d mysql'
        }
      }
  }

}
