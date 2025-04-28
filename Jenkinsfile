pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "my-node-app"
        CONTAINER_NAME = "boring_jemison"
    }

    stages {
        stage('Clone Repository') {
            steps {
                // Clone the repository containing your Dockerfile and app code, explicitly using the 'main' branch
                git branch: 'main', url: 'https://github.com/sakthipriya2326/Node-JS.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    bat 'docker build -t %DOCKER_IMAGE% .'
                }
            }
        }

        stage('Stop and Remove Previous Container') {
            steps {
                script {
                    // Check if the container exists and stop/remove it if it does
                    bat '''
                    if (docker ps -a --filter "name=%CONTAINER_NAME%" --format "{{.Names}}") {
                        docker stop %CONTAINER_NAME%
                        docker rm %CONTAINER_NAME%
                    } else {
                        exit 0
                    }
                    '''
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    // Run the Docker container, map port 4000 of the container to 3000 of the host
                    bat "docker run -d -p 3000:3000 --name %CONTAINER_NAME% %DOCKER_IMAGE%"
                }
            }
        }
    }

    post {
        success {
            echo 'Docker container was successfully built and deployed.'
        }
        failure {
            echo 'There was an error with the build process.'
        }
    }
}
