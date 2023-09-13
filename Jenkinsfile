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
         sh 'docker build -t "10.138.0.3:5001/mgsgoms/mysql:$BUILD_NUMBER"  "$WORKSPACE"/mysql'
         sh 'docker push "10.138.0.3:5001/mgsgoms/mysql:$BUILD_NUMBER"'
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
