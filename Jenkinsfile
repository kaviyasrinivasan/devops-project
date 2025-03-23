pipeline {
    agent any

    stages {
        stage('Clean Workspace') {
            steps {
                echo 'Cleaning workspace...'
                deleteDir()
            }
        }

        stage('Git Checkout') {
            steps {
                script {
                    echo 'Checking out the latest code from GitHub...'
                    git branch: 'main', url: 'https://github.com/kaviyasrinivasan/devops_project.git'
                }
            }
        }

        stage('Install Node') {
            steps {
                script {
                    echo "Checking for package.json in the root directory..."
                    sh 'ls -la'  // Debugging step: Show files

                    if (fileExists('package.json')) {  // ✅ Correct location
                        sh 'npm install'
                    } else {
                        error 'ERROR: package.json is missing. Stopping pipeline.'
                    }
                }
            }
        }

        stage('Build & Test') {
            steps {
                echo 'Running tests...'
                sh 'npm test' // Ensure test scripts are defined in package.json
            }
        }

        stage('Docker Build & Push') {
            steps {
                script {
                    echo 'Building Docker image...'
                    sh 'docker build -t myapp:latest .'
                    
                    echo 'Pushing Docker image to registry...'
                    sh 'docker tag myapp:latest mydockerhubusername/myapp:latest'
                    sh 'docker push mydockerhubusername/myapp:latest'
                }
            }
        }

        stage('Deploy Docker Container') {
            steps {
                script {
                    echo 'Deploying Docker container...'
                    sh 'docker run -d -p 3000:3000 --name myapp_container mydockerhubusername/myapp:latest'
                }
            }
        }
    }
}
