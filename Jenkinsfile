pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "shamanth74/myapp"
    }

    stages {
        // Stage 1: Build (This is already working for you now!)
        stage('Build Docker Image') {
            steps {
                script {
                    // Using the plugin syntax you installed
                    docker.build("${DOCKER_IMAGE}:latest")
                }
            }
        }

        // Stage 2: Login (Fixed for Windows)
        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    // Changed 'sh' to 'bat' and fixed variable syntax for Windows
                    bat "echo %DOCKER_PASS% | docker login -u %DOCKER_USER% --password-stdin"
                }
            }
        }

        // Stage 3: Push (Fixed for Windows)
        stage('Push Docker Image') {
            steps {
                script {
                    // Standard Docker push via batch is more reliable on Windows
                    bat "docker push ${DOCKER_IMAGE}:latest"
                }
            }
        }
    }

    post {
        success {
            echo 'Image successfully built and pushed to Docker Hub'
        }
        failure {
            echo 'Pipeline failed'
        }
    }
}
