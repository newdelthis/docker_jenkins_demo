pipeline {
  agent any

  environment {
    // Define environment variables from credentials
    GIT_REPOSITORY_URL = 'https://github.com/newdelthis/docker_jenkins_demo.git'
    DOCKER_IMAGE_NAME = 'haajkahate/docker_jenkins_demo'
    IMAGE_TAG = '1.0' // Make sure to use string for tag
  }

  stages {
    stage('Clone Repository') {
      steps {
        // Checkout the Git repository with error handling
        script {
          git branch: 'main', url: GIT_REPOSITORY_URL
        }
      }
    }

    stage('Build Docker Image') {
      steps {
        // Build Docker image with error handling
        script {
          docker.build(DOCKER_IMAGE_NAME)
        }
      }
    }

    stage('Login to DockerHub and Push Image') {
      steps {
        // Login to DockerHub with credentials
        script {
          withCredentials([usernamePassword(credentialsId: 'my-docker-hub-credentials-id', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
            sh 'docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD'
          }
        }

        // Push the image to registry
        script {
          docker.withRegistry('https://registry.hub.docker.com') {
            docker.image(DOCKER_IMAGE_NAME).push(IMAGE_TAG)
          }
        }
      }
    }
  }
}
