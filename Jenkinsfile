pipeline {

  environment {
    registry = "sangeetha1501/flask"
    registry_mysql = "sangeetha1501/mysql"
    dockerImage = ""
  }

  agent any
    stages {
  
    stage('Checkout Source') {
      steps {
        git 'https://github.com/SangeeArun1501/Docker-Project.git'
      }
    }

    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
          dockerImage = docker.build registry_mysql + ":$BUILD_NUMBER"
        }
      }
    }

    stage('Push Image') {
      steps{
        script {
          withDockerRegistry([ credentialsId: "sangeethaDockerHub", url: "" ]) {
            dockerImage.push("registry")
            dockerImage.push("registry_mysql")
          }
        }
      }
    }

    stage('current') {
      steps{
        dir("${env.WORKSPACE}/mysql"){
          sh "pwd"
          }
      }
   }

    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "frontend.yaml", kubeconfigId: "kube")
        }
      }
    }

  }

}
