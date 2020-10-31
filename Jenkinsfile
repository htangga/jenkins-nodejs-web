pipeline {

  environment {
    registry = 'harbor.unimed.ac.id/library/nodejs-web'
    registryHost = 'https://harbor.unimed.ac.id'
    registryCredentials = 'harbor'
    dockerImage = ''
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git 'https://github.com/htangga/jenkins-play.git'
      }
    }

    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }

    stage('Push Image') {
      steps{
        script {
          docker.withRegistry( registryHost , registryCredentials ) {
            dockerImage.push()
          }
        }
      }
    }

    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "hpa.yaml", kubeconfigId: "unimed-kubeconfig")
        }
      }
    }

  }

}
