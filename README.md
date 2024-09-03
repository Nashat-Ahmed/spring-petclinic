
# Spring PetClinic CI/CD Pipeline with Docker and Docker Hub

This repository contains a CI/CD pipeline that Dockerizes the Spring PetClinic application and pushes the Docker image to Docker Hub.

## Prerequisites

Before you start, ensure you have the following installed:

- [Docker](https://www.docker.com/get-started)
- [Jenkins](https://www.jenkins.io/download/) (if you are using Jenkins for the pipeline)
- [Git](https://git-scm.com/downloads)
- A [Docker Hub](https://hub.docker.com/) account

## Pipeline Overview
The pipeline is structured to perform the following steps:

1. **Clone the repository**: Fetch the source code of the Spring PetClinic application.
2. **Build the application**: Compile the source code using Maven.
3. **Dockerize the application**: Create a Docker image for the application.
4. **Push to Docker Hub**: Push the Docker image to Docker Hub.

## Running the Pipeline

### git clone https://github.com/Nashat-Ahmed/spring-petclinic.git

### Create Jenkins Credentials: Go to Jenkins > Manage Jenkins > Manage Credentials and add Docker Hub credentials with ID `docker-hub-credentials-id`.

###  Create a new pipeline job in Jenkins, point it to your repository, and run the pipeline.



