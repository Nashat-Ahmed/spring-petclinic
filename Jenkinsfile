pipeline {
    agent any

    environment {
        DOCKER_HUB_REPO = 'nashaat111/spring-petclinic'
        DOCKER_CREDENTIALS_ID = 'docker-hub-repo'
        DEPLOY_SERVER = '35.170.182.83'
        DEPLOY_USER = 'ec2-user'
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
                    sshagent(['ec2-server-key']) {
                        sh """
                        scp docker-compose.yml ${DEPLOY_USER}@${DEPLOY_SERVER}:/home/${DEPLOY_USER}/docker-compose.yml
                        ssh -o StrictHostKeyChecking=no ${DEPLOY_USER}@${DEPLOY_SERVER} '
                            docker-compose -f /home/${DEPLOY_USER}/docker-compose.yml up -d --remove-orphans
                        '
                        """
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
