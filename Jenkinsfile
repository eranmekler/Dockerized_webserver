pipeline {
  agent any
  triggers {
    githubPush()
  }
  stages {
    stage('Build and Tag Image') {
      steps {
        script {
          sh """
            git_tag=$(git describe --abbrev=0 --tags 2>/dev/null || echo "latest")
            echo "Building image with tag: ${git_tag}"
            docker build -t my-app-image:${git_tag} .
          """
        }
      }
    }

    stage('Push to Docker Hub') {
      steps {
        script {
          withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
            sh "docker push ${env.IMAGE_NAME}:${gitTag}"
            sh "docker push ${env.IMAGE_NAME}:latest"
          }
        }
      }
    }
  }
}
