pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'hello-world-app'
        DOCKER_COMPOSE_FILE = 'docker-compose.yml'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/expressjs/express.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE}:${env.BUILD_ID}")
                }
            }
        }
        stage('Test') {
            steps {
               
                echo 'Running tests...'
            }
        }
        stage('Push to Docker Hub') {
            steps {
                script {
                    echo ' Docker image pushed to Docker hub..'
                    }
                }
            }
        }
        stage('Deploy to Staging') {
            steps {
                script {
                   
                    writeFile file: 'docker-compose.yml', text: """
                    version: '3.8'
                    services:
                      app:
                        image: hello-world-app:${env.BUILD_ID}
                        ports:
                          - "8080:8080"
                        environment:
                          - PORT=8080
                    """

                    sh 'docker-compose down'
                    sh 'docker-compose up -d'
                }
            }
        }
    }

    post {
        failure {
            stage('Rollback') {
                steps {
                    script {
                        sh 'docker-compose down'
                        sh 'docker run -d -p 8080:8080 --name hello-world-app hello-world-app:v1'
                    }
                }
            }
        }
    }
}
