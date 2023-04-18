pipeline {
  
  environment {
    DOCKERHUB_CREDENTIALS = credentials('dockerhub-credentials')
    // dockerimagename = "adilson-tavares/react-app"
    // dockerImage = ""
  }
  
  agent {
      docker { image 'node:16.13.1-alpine' }
  }
  stages {

    stage('Checkout Source') {
      steps {
        git branch: 'main', url: 'https://github.com/adilson-tavares/sampl-react-jenkins-deploy.git',
        credentialsId: 'github-credentials'
      }
    }
    // stage('Initialize'){
    //       def dockerHome = tool '/home/adilson/.docker/'
    //       env.PATH = "${dockerHome}/bin:${env.PATH}"
    // }
    stage('Build image') {
      steps {
        // dockerImage = docker.build("${env.dockerimagename}")
        sh 'docker build -t tavarescruz/reactjs-app .'
      }
    }

    stage('Login') {
      steps {
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
      }
    }

    stage('Pushing Image') {

      steps {
        // sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
        sh 'docker push tavarescruz/reactjs-app-docker'
      }
      // environment {
      //          registryCredential = 'dockerhub-credentials'
      //      }
      // steps{
      //   script {
      //     docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
      //       dockerImage.push("latest")
      //     }
      //   }
      // }
    }

    stage('Deploying React.js container to Kubernetes') { 
      steps {
        script {
          kubernetesDeploy(configs: "deployment.yaml", "service.yaml")
        }
      }
    }

  }
  post {
    always {
      sh 'docker logout'
    }
  }
}