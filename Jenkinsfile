pipeline {
    agent any

    environment {
        DOCKER_HUB_REPO = 'nashaat111/spring-petclinic'
        DOCKER_CREDENTIALS_ID = 'docker-hub-repo'
    }

    stages {
        stage('Build Application') {
            steps {
                // Use Maven to build the application
                sh 'mvn clean package'
            }
        }

        stage('Docker Build') {
            steps {
                script {
                    // Build Docker image using Dockerfile
                    sh "docker build -t ${DOCKER_HUB_REPO}:latest ."
                }
            }
        }

        stage('Docker Login') {
            steps {
                script {
                    // Login to Docker Hub
                    withCredentials([usernamePassword(credentialsId: "${DOCKER_CREDENTIALS_ID}", passwordVariable: 'DOCKER_HUB_PASSWORD', usernameVariable: 'DOCKER_HUB_USERNAME')]) {
                        sh 'echo "$DOCKER_HUB_PASSWORD" | docker login -u "$DOCKER_HUB_USERNAME" --password-stdin'
                    }
                }
            }
        }

        stage('Docker Push') {
            steps {
                script {
                    // Push Docker image to Docker Hub
                    sh "docker push ${DOCKER_HUB_REPO}:latest"
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Deploy using Docker Compose
                    def dcokerComposeCmd = "docker-compose -f docker-compose.yml up --detach"
                    sshagent(['ec2-server-key']) {
                        sh "ssh  ec2-user@35.170.182.83 ${dockerComposeCmd}"
                    }
                }
            }
        }
    }

    post {
        always {
            // Cleanup Docker images to free space
            sh "docker rmi ${DOCKER_HUB_REPO}:latest || true"
        }
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
