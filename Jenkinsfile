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
          docker.image.build("-t esmailshaikh1055/jenkins .")
       }
      }
    }
       stage('Login') {
  steps {
    // Login to Docker Hub
    script {
      docker.withRegistry('https://index.docker.io/v1/', DOCKERHUB_CREDENTIALS) {
        // Login using credentials
        echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin
      }
    }
  }
}

    stage('Push') {
      steps {
        // Push Docker image to Docker Hub
        script {
          docker image push esmailshaikh1055/jenkins
        }
      }
    }
    stage('Create Container') {
      steps {
        // Run the Docker container
        script {
          docker.container("run -itd -p 86:80 --name mycontainer esmailshaikh1055/jenkins")
        }
      }
    }
  }
  post {
    always {
      // Logout from Docker
      script {
        docker logout
      }
    }
  }
}

