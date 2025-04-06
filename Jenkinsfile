pipeline {
    agent any

    environment {
        IMAGE_NAME = 'susindraa/java-maven-app'
        DOCKER_CREDENTIALS_ID = 'docker-id'          // DockerHub credentials
        CONTAINER_NAME = 'java-maven-container'       // Name for the running container
        HOST_PORT = '9090'                            // Port on host (to avoid conflict with Jenkins)
        CONTAINER_PORT = '8080'                       // App port inside the container
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'master', credentialsId: 'GitHubCreds', url: 'https://github.com/susindraa/MavenBuild.git'
            }
        }

        stage('Build Maven App') {
            steps {
                bat 'mvn clean install'
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

        stage('Deploy Container') {
            steps {
                script {
                    echo "Stopping old container if it exists..."
                    bat "docker rm -f ${CONTAINER_NAME} || echo No container to remove"

                    echo "Running new container from image..."
                    bat "docker run -d -p ${HOST_PORT}:${CONTAINER_PORT} --name ${CONTAINER_NAME} ${IMAGE_NAME}"
                }
            }
        }
    }
}
