pipeline {
    agent any
    environment {
        DOCKER_CRED = credentials('dockerhub')  // Jenkins credential ID for Docker Hub
        DOCKER_IMAGE_FLASK = 'sangeetha1501/flaskapp'
        DOCKER_IMAGE_MYSQL = 'sangeetha1501/mysql'
    }
    stages {
        stage('Checkout Source') {
            steps {
                echo 'Checking out the source code...'
                git 'https://github.com/SangeeArun1501/Docker-Project.git'
            }
        }
        stage('Build code') {
            steps {
                script {
                    echo 'Building Flask image...'
                    def flaskImage = docker.build(DOCKER_IMAGE_FLASK)

                    echo 'Building MySQL image...'
                    dir('mysql') {
                        def mysqlImage = docker.build(DOCKER_IMAGE_MYSQL)
                        // Store mysqlImage in a global variable to access later
                        currentBuild.rawBuild.getAction(hudson.model.ParametersAction.class)?.getParameters().add(new hudson.model.StringParameterValue('mysqlImage', mysqlImage.imageName))
                    }

                    echo 'Image Build Complete'
                    echo "Flask Image: ${flaskImage.imageName}"
                }
            }
        }
        stage('Push to Docker Hub') {
            steps {
                script {
                    echo 'Starting Docker push process...'

                    // Get the MySQL image name from parameters
                    def mysqlImage = currentBuild.rawBuild.getAction(hudson.model.ParametersAction.class)?.getParameters()?.find { it.name == 'mysqlImage' }?.value

                    withDockerRegistry([ credentialsId: 'dockerhub', url: '' ]) {
                        try {
                            echo 'Pushing Flask image...'
                            flaskImage.push()
                            echo 'Flask image pushed successfully.'
                        } catch (Exception e) {
                            echo "Error pushing Flask image: ${e.getMessage()}"
                        }

                        try {
                            echo 'Pushing MySQL image...'
                            if (mysqlImage) {
                                docker.image(mysqlImage).push()
                                echo 'MySQL image pushed successfully.'
                            } else {
                                echo 'MySQL image not available to push.'
                            }
                        } catch (Exception e) {
                            echo "Error pushing MySQL image: ${e.getMessage()}"
                        }
                    }
                }
            }
        }
    }
    post {
        always {
            echo 'Pipeline completed.'
        }
    }
}
