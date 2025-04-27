pipeline {
    agent {
        dockerfile {
            filename 'dockerfile'  
            dir '.'               
        }
    }

    triggers {
        githubPush() 
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm  
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'  
            }
        }

        stage('Run Tests') {
            steps {
                
                echo 'Running app tests...'
                sh 'npm start &'
                sh 'sleep 5' 
                sh 'curl http://localhost:3000' 
            }
        }

        stage('Build Complete') {
            steps {
                echo 'Build and test complete!'  
            }
        }
    }
}
