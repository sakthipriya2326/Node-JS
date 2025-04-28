pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "my-node-app"
        CONTAINER_NAME = "boring_jemison"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/sakthipriya2326/Node-JS.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    bat "docker build -t %DOCKER_IMAGE% ."
                }
            }
        }

        stage('Stop and Remove Previous Container') {
            steps {
                script {
                    bat '''
                    FOR /F "tokens=*" %%i IN ('docker ps -a -q -f "name=%CONTAINER_NAME%"') DO docker rm -f %%i
                    '''
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    bat "docker run -d -p 3000:3000 --name %CONTAINER_NAME% %DOCKER_IMAGE%"
                }
            }
        }
    }

    post {
        failure {
            echo 'There was an error with the build process.'
        }
    }
}
