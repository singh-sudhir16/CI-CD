pipeline {
    agent any

    environment {
        IMAGE_NAME = 'singhsa16/my-node-app'
        IMAGE_TAG = '1.0'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }

        stage('Test') {
            steps {
                bat 'npm test'
            }
        }

        stage('Build') {
            steps {
                bat 'npm run build'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat "docker build -t %IMAGE_NAME%:%IMAGE_TAG% ."
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'docker_cred',
                    usernameVariable: 'DOCKERHUB_USERNAME',
                    passwordVariable: 'DOCKERHUB_PASSWORD'
                )]) {
                    bat '''
                        echo %DOCKERHUB_PASSWORD% | docker login -u %DOCKERHUB_USERNAME% --password-stdin
                        docker tag %IMAGE_NAME%:%IMAGE_TAG% %IMAGE_NAME%:%IMAGE_TAG%
                        docker push %IMAGE_NAME%:%IMAGE_TAG%
                        docker logout
                    '''
                }
            }
        }
    }

    post {
        success {
            echo '✅ Build and Push successful.'
        }
        failure {
            echo '❌ Pipeline failed.'
        }
    }
}
