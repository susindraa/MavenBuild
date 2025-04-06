 pipeline {
    agent any

    environment {
        IMAGE_NAME = 'susindraa/java-maven-app'
        DOCKER_CREDENTIALS_ID = 'docker-id'   // your DockerHub credential ID in Jenkins
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'master', url: 'https://github.com/YOUR_USERNAME/YOUR_REPO.git'
            }
        }

        stage('Build Maven App') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${IMAGE_NAME}", '.')
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', "${DOCKER_CREDENTIALS_ID}") {
                        docker.image("${IMAGE_NAME}").push('latest')
                    }
                }
            }
        }
    }
}
