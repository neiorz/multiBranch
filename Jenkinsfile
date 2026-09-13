pipeline {
    agent any
    environment {
        DOCKER_IMAGE = 'nourz1/nodejs-docker-exercise'
        CREDENTIALS_ID = 'docker'
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    app = docker.build("${DOCKER_IMAGE}:${env.BRANCH_NAME}-${env.BUILD_NUMBER}")
                }
            }
        }
        
        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', "${CREDENTIALS_ID}") {
                        app.push("latest")
                        app.push("${env.BRANCH_NAME}-${env.BUILD_NUMBER}")
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f deployment.yaml'
            }
        }
    }
}
