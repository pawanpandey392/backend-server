pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "node-app"
        DOCKER_TAG = "latest"
    }

    stages {
        stage('Clone Repository') {
            steps {
                // Clone the repository from Git
                git 'https://github.com/username/node-app.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                // Run npm install to install dependencies
                sh 'npm install'
            }
        }

        stage('Test') {
            steps {
                // Run tests
                sh 'npm test'
            }
        }

        stage('Build Docker Image') {
            steps {
                // Build Docker image
                script {
                    docker.build("${DOCKER_IMAGE}:${DOCKER_TAG}")
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                // Stop and remove the previous container if it exists
                script {
                    sh 'docker stop node-app || true'
                    sh 'docker rm node-app || true'

                    // Run the new Docker container
                    sh 'docker run -d --name node-app -p 3000:3000 ${DOCKER_IMAGE}:${DOCKER_TAG}'
                }
            }
        }
    }

    post {
        always {
            echo "Cleaning up Docker images"
            sh 'docker system prune -f'
        }
    }
}
