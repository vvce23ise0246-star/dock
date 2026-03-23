pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "srajan106/myapp"
    }

    stages {

        stage('Clone Repository') {
            steps {
                git 'https://github.com/vvce23ise0246-star/dock.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE}:latest")
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('', 'dockerhub-creds') {
                        docker.image("${DOCKER_IMAGE}:latest").push()
                    }
                }
            }
        }
        stage('Clone Repository') {
    steps {
        git branch: 'main', url: 'https://github.com/vvce23ise0246-star/dock.git'
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
