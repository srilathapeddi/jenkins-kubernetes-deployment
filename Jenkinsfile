pipeline {
  environment {
    dockerimagename = "srilatha7842/react-app"
    dockerImage = ""
    DOCKER_TLS_CERTDIR="/certs"
    //DOCKER_TLS_VERIFY=0
    //DOCKER_CERT_PATH = "C:/Users/PC/.minikube/certs" // Unset the DOCKER_CERT_PATH to avoid using non-existent certificates
    DOCKER_HOST="tcp://docker:2376"
    DOCKER_CERT_PATH="/certs/client"
    DOCKER_TLS_VERIFY=1
    //DOCKER_TLS_CERTDIR="C:/Users/PC/.minikube/certs"
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
          //dockerImage = docker.build dockerimagename
          dockerImage = docker.build(dockerimagename, '-f Dockerfile .')
        }
      }
    }
    stage('Pushing Image ') {
      environment {
          registryCredential = 'srilatha7842'
           }
      steps{
        script {
          docker.withRegistry('https://registry.hub.docker.com', registryCredential) {
            sh "echo 'Docker login successfull'"
            dockerImage.push("v1")
          }
        }
      }
    }
    stage('Deploying React.js container to Kubernetes') {
      steps {
        script {
          // kubernetesDeploy(configs: "deployment.yaml", 
          //                                "service.yaml")
          kubernetes{
            yamlFile "deployment.yaml"
            yamlFile "service.yaml"
          }
        }
        
      }
    }
  }
}