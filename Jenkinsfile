pipeline {
    agent any
    environment {
        DOCKER_HUB_CREDENTIALS = credentials('dockerhub-creds') // Jenkins credential ID
        IMAGE_NAME = "taayabbb/salary-predictor"
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/m-tayyab-naseem/mlops-salary-predictor-model.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                bat "docker build -t %IMAGE_NAME%:%BUILD_NUMBER% ."
            }
        }
        stage('Login & Push') {
            steps {
                // Windows doesn't support "echo password | docker login"
                // Instead, use -p flag
                bat "docker login -u %DOCKER_HUB_CREDENTIALS_USR% -p %DOCKER_HUB_CREDENTIALS_PSW%"
                bat "docker push %IMAGE_NAME%:%BUILD_NUMBER%"
            }
        }
    }
    post {
        success {
            mail to: 'tayyabnassem246@gmail.com',
                 subject: "Build Successful: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                 body: "Docker image pushed to %IMAGE_NAME%:%BUILD_NUMBER%"
        }
        failure {
            mail to: 'tayyabnassem246@gmail.com',
                 subject: "Build Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                 body: "Please check the Jenkins logs."
        }
    }
}
