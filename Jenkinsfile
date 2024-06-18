pipeline {
  agent any

  scm {
    git url: 'https://github.com/eranmekler/Dockerized_webserver.git'
    credentialsId: 'ssh_key_for_github'
    fetchTags true
  }

  triggers {
    githubPush events: 'push', branches: [ 'main' ]
  }
  stages {
    stage('Build and Tag Image') {
      steps {
        script {
          def gitTag = sh(script: 'git describe --abbrev=0 --tags', returnStdout: true).trim()

          echo "Building and tagging image with version: ${gitTag}"

          sh "docker build -t ${env.IMAGE_NAME}:${gitTag} ."
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
