pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        script {
          sh 'docker build -t nashaat111/webapp:1.3 .'
        }
      }
    }
    stage('Test') {
      steps {
        script {
          sh 'docker run --rm nashaat111/webapp:1.3 npm test'
        }
      }
    }
    stage('Deploy') {
      steps {
        script {
          sh 'docker run -d -p 3000:3000 nashaat111/webapp:1.3'
        }
      }
    }
    stage('Pushing to dockerhup') {
      steps {
        script {
          sh 'docker push nashaat111/webapp:1.3'
        }
      }
    }

  }
}

