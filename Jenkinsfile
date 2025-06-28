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
	stage('Build Image'){
	    steps {
		sh 'docker build -t my-node-app:1.0 .'
	    }
	}
    }
}
