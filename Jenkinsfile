pipeline {

  environment {
    registry = "sangeetha1501/flask"
    registry_mysql = "sangeetha1501/mysql"
    dockerImage = ""
    newImage = ""
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
        }
      }
    }

    stage('Push Image') {
      steps{
        script {
          withDockerRegistry([ credentialsId: "sangeethaDockerHub", url: "" ]) {
            dockerImage.push("registry")
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
    stage('Build mysql image') {
      steps{
         newImage = docker.build registry_mysql + ":$BUILD_NUMBER"
        withDockerRegistry([ credentialsId: "sangeethaDockerHub", url: "" ])
         newImage.push("registry_mysql")
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
