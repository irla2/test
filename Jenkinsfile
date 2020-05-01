#pipeline {
#    agent {
#        docker {
#            image 'node:6-alpine'
#            args '-p 3000:3000'
#        }
#    }
#    stages {
#        stage ('Build'){
#            steps {
#                sh 'npm install'
#            }
#        }
#    }
#}

pipeline {
  environment {
    registry = "localhost:5000"
    registryCredential = 'dockerhub'
    dockerImage = ''
  }
  agent any
  stages {
    stage('Cloning Git') {
      steps {
        git 'https://github.com/irla2/test.git'
      }
    }
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }
    stage('Deploy Image') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
          }
        }
      }
    }
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $registry:$BUILD_NUMBER"
      }
    }
  }
}
