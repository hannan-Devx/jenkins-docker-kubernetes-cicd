pipeline {

    agent any

    environment {
        IMAGE_NAME = "abdulhannan019/htmlimage"
        IMAGE_TAG = "v${BUILD_NUMBER}"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/hannan-Devx/jenkins-docker-kubernetes-cicd.git'
            }
        }


        stage('Build Docker Image') {
            steps {
                bat """
                docker build -t %IMAGE_NAME%:%IMAGE_TAG% .
                """
            }
        }


        stage('Docker Login & Push') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-creds',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASS'
                    )
                ]) {

                    bat """
                    docker login -u %DOCKER_USER% -p %DOCKER_PASS%
                    docker push %IMAGE_NAME%:%IMAGE_TAG%
                    """
                }
            }
        }


        stage('Deploy to Kubernetes') {
            steps {

                bat """
                kubectl set image deployment/html-deployment html-container=%IMAGE_NAME%:%IMAGE_TAG%
                """
            }
        }
    }
}