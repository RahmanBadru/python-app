pipeline {
  environment {
    dockerimagename = "rahmanbadru/python-app"
    dockerImage = ""
  }
  agent any
  stages {
    stage('Checkout Source') {
      steps {
        git branch: 'main', credentialsId: 'github creds', url:'https://github.com/RahmanBadru/python-app.git'
      }
    }
    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build dockerimagename
        }
      }
    }
    stage('Pushing Image') {
      environment {
          registryCredential = 'dockerhub creds'
           }
      steps{
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
            dockerImage.push("latest")
          }
        }
      }
    }
    stage('Deploying Python container to Kubernetes') {
      withKubeConfig([credentialsId: 'my_kubernetes', serverUrl: 'https://127.0.0.1:32769']) {
        sh 'kubectl apply -f deployment.yaml service.yaml'
      }
    }
  }
}
