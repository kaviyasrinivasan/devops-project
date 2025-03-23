pipeline {
    agent any

    environment {
        APP_NAME = "react-app"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/kaviyasrinivasan/devops-project.git'  // Update with your repo URL
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build React App') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm test'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ${APP_NAME}:latest .'
            }
        }

        stage('Run Docker Container') {
            steps {
                sh 'docker run -d -p 80:80 --name ${APP_NAME} ${APP_NAME}:latest'
            }
        }
    }

    post {
        success {
            echo 'Build and deployment successful!'
        }
        failure {
            echo 'Build failed! Check logs for errors.'
        }
    }
}
