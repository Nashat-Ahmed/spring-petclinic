pipeline {
  agent any
  stages {
   stage('Test') {
      steps {
        script {
          sh 'git clone https://github.com/spring-projects/spring-petclinic.git'
        }
      }
    }
    stage('Build') {
      steps {
        script {
          sh 'docker build -t nashaat111/webapp:1.2 .'
        }
      }
    }
    stage('Test') {
      steps {
        script {
          sh 'docker run --rm nashaat111/webapp:1.2 npm test'
        }
      }
    }
    stage('Deploy') {
      steps {
        script {
          sh 'docker run -d -p 3000:3000 nashaat111/webapp:1.2'
        }
      }
    }
    stage('Pushing to dockerhup') {
      steps {
        script {
          sh 'docker push nashaat111/webapp:1.2'
        }
      }
    }

  }
}

