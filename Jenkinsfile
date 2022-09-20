pipeline {

  environment {
    dockerimagename = "madhublue01/nodeapp"
    dockerImage = ""
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git 'https://github.com/shazforiot/nodeapp_test.git'
      }
    }

    stage('Build image') {
      steps{
        script {
          sh 'docker build -t madhublue01/nodeapp .'
        }
      }
    }

    stage('Pushing Image') {
      environment {
               registryCredential = 'dockerhublogin'
           }
      steps{
        script {
          withCredentials([string(credentialsId: 'dockerhublogin', variable: 'dockerhublogin')]) {
            sh 'docker login -u madhublue01 -p ${dockerhublogin}'
          }
        }
      }
    }

    stage('Deploying App to Kubernetes') {
      steps {
        script {
          kubernetesDeploy (configs: 'deploymentservice.yml',kubeconfigId: 'Kubeadm')
        }
      }
    }

  }

}
