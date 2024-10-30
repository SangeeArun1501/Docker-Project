pipeline {
    agent any

    environment {
        SONARQUBE_SERVER = 'http://34.48.73.26:9000'
        SONARQUBE_TOKEN = credentials('sonarqube')
        PATH = "${PATH}:/opt/sonar-scanner/bin"
        DOCKER_CREDENTIALS = 'dockerhub'
        DOCKER_USERNAME = 'sangeetha1501'
        KUBECONFIG = '/home/sakshara479/jenkins/kubeconfig'
    }

    stages {
        stage('Scan and build backend image') {
            steps {
                script {
                    echo "Starting backend build..."
                    git branch: 'sangeetha-backend', 
                        url: 'https://github.com/cubensquare/container-cost-management.git', 
                        credentialsId: 'github'
                    withCredentials([string(credentialsId: 'sonarqube', variable: 'SONARQUBE_TOKEN')]) {
                        sh """
                            sonar-scanner \
                            -Dsonar.projectKey=jenkins_integration \
                            -Dsonar.sources=. \
                            -Dsonar.host.url=${SONARQUBE_SERVER} \
                            -Dsonar.login=${SONARQUBE_TOKEN} \
                            -Dsonar.java.binaries=target/classes
                        """
                    }
                    sh 'docker build -t ${DOCKER_USERNAME}/backend .'
                    echo "Finished backend build."
                }
            }
        }
        stage('Scan and build frontend image') {
            steps {
                script {
                    echo "Starting frontend build..."
                    git branch: 'sangeetha-frontend', 
                        url: 'https://github.com/cubensquare/container-cost-management.git', 
                        credentialsId: 'github'
                    withCredentials([string(credentialsId: 'sonarqube', variable: 'SONARQUBE_TOKEN')]) {
                        sh """
                            sonar-scanner \
                            -Dsonar.projectKey=frontend \
                            -Dsonar.sources=. \
                            -Dsonar.host.url=${SONARQUBE_SERVER} \
                            -Dsonar.login=${SONARQUBE_TOKEN} \
                            -Dsonar.java.binaries=target/classes
                        """
                    }
                    sh 'docker build -t ${DOCKER_USERNAME}/frontend .'
                    echo "Finished frontend build."
                }
            }
        }
        stage('Test Push Stage') {
            steps {
                echo "This is a test stage before pushing."
            }
        }
        stage('Push to Dockerhub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', DOCKER_CREDENTIALS) {
                        sh 'docker push ${DOCKER_USERNAME}/backend'
                        sh 'docker push ${DOCKER_USERNAME}/frontend'
                    }
                }
            }
        }
        stage('Check Kubernetes Connection and Deploy') {
            steps {
                script {
                    echo "Checking Kubernetes connection..."
                    sh 'kubectl get nodes --kubeconfig=${KUBECONFIG}'
                    echo "Kubernetes connection successful."

                    echo "Checking out the backend branch for deployment..."
                    git branch: 'sangeetha-backend', 
                        url: 'https://github.com/cubensquare/container-cost-management.git', 
                        credentialsId: 'github'

                    echo "Deploying to Kubernetes..."
                    sh 'kubectl apply -f configmap.yaml --kubeconfig=${KUBECONFIG}'
                    sh 'kubectl apply -f secret.yaml --kubeconfig=${KUBECONFIG}'
                    sh 'kubectl apply -f mysql.yaml --kubeconfig=${KUBECONFIG}'
                    sh 'kubectl apply -f springboot.yaml --kubeconfig=${KUBECONFIG}'
                    sh 'kubectl apply -f frontend.yaml --kubeconfig=${KUBECONFIG}'
                    
                    
                    echo "Deployment to Kubernetes completed."
                }
            }
        }
    }
}
