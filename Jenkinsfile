pipeline {

  environment {
    dockerimagename = "adilson-tavares/react-app"
    dockerImage = ""
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git 'https://github.com/adilson-tavares/sampl-react-jenkins-deploy.git'
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
               registryCredential = 'dockerhub-credentials'
           }
      steps{
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
            dockerImage.push("latest")
          }
        }
      }
    }

    stage('Deploying React.js container to Kubernetes') { 
      steps {
        script {
          kubernetesDeploy(configs: "service/deployment.yaml", "service/service.yaml")
        }
      }
    }

  }

}