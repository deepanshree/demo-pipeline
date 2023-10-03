pipeline {
    agent any

    environment {
        // Define environment variables for your Docker image
        DOCKER_IMAGE_NAME = 'my-docker-image'
        DOCKERFILE_PATH = "${WORKSPACE}/Dockerfile" // Use the Jenkins workspace
        CONTAINER_NAME = 'my-docker-container'
        HOST_PORT = '8091' // Port on the host machine
        CONTAINER_PORT = '8080' // Port exposed by the container
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from your Git repository
                git(url: 'https://github.com/deepanshree/demo-pipeline.git', branch: 'main')
            }
        }

        stage('Build Docker Image') {
            steps {
                // Build the Docker image
                script {
                    def customImage = docker.build(DOCKER_IMAGE_NAME, "-f ${DOCKERFILE_PATH} .")
                }
            }
        }
    
        stage('Stop and Remove Existing Container') {
            steps {
                script {
                        // Check if the container exists
                        bat "docker rm -f ${CONTAINER_NAME}"
                }
            }
        }        

        stage('Deploy Docker Container') {
            steps {
                // Run the Docker container from the image using a Windows shell command
                bat """
                    docker run -d -p ${HOST_PORT}:${CONTAINER_PORT} --name ${CONTAINER_NAME} ${DOCKER_IMAGE_NAME}
                """
            }
        }
    }
}
