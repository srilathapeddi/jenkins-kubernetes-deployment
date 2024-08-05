pipeline {
  environment {
    dockerimagename = "srilathapeddi/react-app"
    dockerImage = ""
    //DOCKER_TLS_VERIFY=0
    DOCKER_CERT_PATH = 'C:/Program Files/Git/usr/ssl' // Unset the DOCKER_CERT_PATH to avoid using non-existent certificates
  }
  agent any
  stages {
    stage('Checkout Source') {
      steps {
        git branch: 'main', url: 'https://github.com/srilathapeddi/jenkins-kubernetes-deployment.git'
      }
    }
    stage('Build image') {
      steps{
        script {
          //unset DOCKER_CERT_PATH
          //export DOCKER_TLS_VERIFY=0
          dockerImage = docker.build dockerimagename
          //dockerImage = docker.build(dockerimagename, '-f jenkins_deploy/Dockerfile .')
        }
      }
    }
    stage('Pushing Image ') {
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
          kubernetesDeploy(configs: "deployment.yaml", 
                                         "service.yaml")
        }
      }
    }
  }
}