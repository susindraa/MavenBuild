pipeline {
    agent any

    environment {
        IMAGE_NAME = 'susindraa/java-maven-app'
        DOCKER_CREDENTIALS_ID = 'docker-id'   // DockerHub credentials
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'master', credentialsId: 'GitHubCreds', url: 'https://github.com/susindraa/MavenBuild.git'
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
