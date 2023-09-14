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
       sh 'docker build -t "sangeetha1501/mysql:$BUILD_NUMBER"  "$WORKSPACE"/mysql'
       withDockerRegistry([ credentialsId: "sangeethaDockerHub", url: "" ]) {
        sh 'docker push "sangeetha1501/mysql:$BUILD_NUMBER" '
        }
      }
    }
    stage('Deploy App') {
      steps {
        script {
         withKubeConfig([credentialsId: 'kube']) {
         sh 'kubectl apply -f frontend.yaml'
    }
        }
      }
    }

  }

}
