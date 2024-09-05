pipeline {
    agent any

    environment {
        DOCKER_HUB_REPO = 'nashaat111/spring-petclinic'
        DOCKER_CREDENTIALS_ID = 'docker-hub-repo'
        DEPLOY_SERVER = '44.204.4.75'  
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
                    // Build Docker image
                    sh 'docker build -t ${DOCKER_HUB_REPO}:latest .'
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
                    sh 'docker push ${DOCKER_HUB_REPO}:latest'
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Deploy the Docker image to the remote server
                    sshagent(['ec2-server-key']) {  
                        sh """
                        ssh -o StrictHostKeyChecking=no ${DEPLOY_USER}@${DEPLOY_SERVER} 'docker pull ${DOCKER_HUB_REPO}:latest && docker stop petclinic || true && docker rm petclinic || true && docker run -d --name petclinic -p 8080:8080 ${DOCKER_HUB_REPO}:latest'
                        """
                    }
                }
            }
        }
    }

    post {
        always {
            // Cleanup Docker images to free space
            sh 'docker rmi ${DOCKER_HUB_REPO}:latest || true'
        }
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
