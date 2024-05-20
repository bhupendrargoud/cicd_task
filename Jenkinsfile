pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'hello-world-app'
        DOCKER_COMPOSE_FILE = 'docker-compose.yml'
    }

    stages {
        stage('Clone Repository') {
        steps {
            git branch: 'main', url: 'https://github.com/bhupendrargoud/cicd_task.git'
            }
        }
        
            stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${DOCKER_IMAGE}:${env.BUILD_ID} ."
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
    
        stage('Deploy to Staging') {
            steps {
                script {
                   
                    writeFile file: 'docker-compose.yml', text: """
                    version: '3.8'
                    services:
                      app:
                        image: hello-world-app:${env.BUILD_ID}
                        ports:
                        - "8080:3000"
                        environment:
                        - PORT=3000
                    """

                    sh 'docker-compose down'
                    sh 'docker-compose up -d'
                }
            }
        }
    }

   post {
    
    failure {
        script {
            def lastSuccessfulBuild = currentBuild.previousSuccessfulBuild
                    if (lastSuccessfulBuild != null) {
                        def previousBuildNumber = lastSuccessfulBuild.getNumber()
                        echo "Rolling back to previous build: $previousBuildNumber"

             writeFile file: 'docker-compose.yml', text: """
                    version: '3.8'
                    services:
                      app:
                        image: hello-world-app:${previousBuildNumber}
                        ports:
                        - "8080:3000"
                        environment:
                        - PORT=3000
                    """

                    sh 'docker-compose down'
                    sh 'docker-compose up -d'
           
                    } else {
                        echo "No previous successful builds found."
                    }
            
           
            }
        }
    }

}