pipeline {
  agent any
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  environment {
    DOCKERHUB_CREDENTIALS = credentials('esmail1055-shaikh')
  }
  stages {
    stage('Build') {
      steps {
        script {
          // Build the Docker image
          docker.image('esmailshaikh1055/jenkins').build()
        }
      }
    }
    stage('Login') {
      steps {
        // Login to Docker Hub
        script {
          docker.withRegistry('https://index.docker.io/v1/', DOCKERHUB_CREDENTIALS) {
            // Login using credentials
            docker.image('esmailshaikh1055/jenkins').push()
          }
        }
      }
    }
    stage('Push') {
      steps {
        // Push Docker image to Docker Hub
        script {
          docker.image('esmailshaikh1055/jenkins').push()
        }
      }
    }
    stage('Create Container') {
      steps {
        // Run the Docker container
        script {
          docker.container('mycontainer').withRun('-itd -p 86:80 esmailshaikh1055/jenkins')
        }
      }
    }
  }
  post {
    always {
      // Logout from Docker
      script {
        docker.image('esmailshaikh1055/jenkins').registry().credentialsId(DOCKERHUB_CREDENTIALS).logout()
      }
    }
  }
}

