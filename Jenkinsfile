
//
// define variables
//

pipeline {
  environment {
    tag = "$GIT_BRANCH"
    imagenamebase = "pwunix/h2ogpt"
    imagename = "${imagenamebase}:${tag}"
    registryCredential = ''
    dockerImage = ''
    dockerregistry='https://registry.pathcom.com:5000'
    dockercredential=''
  }
  agent any
  stages {
//    stage('Cloning Git') {
//      steps {
//        git ''
//      }
//    }
    stage('Prepare Build') {
      steps {
        sh 'git rev-parse HEAD > git_hash.txt'
        sh 'touch build_info.txt'	
      }
    }
    stage('Building image') {
      steps{
        script {
	  docker.withRegistry("${env.dockerregistry}") {
            dockerImage = docker.build("${env.imagename}")
	  }
        }
      }
    }
    stage('Deploy Image') {
      steps{
        script {
	   docker.withRegistry("${env.dockerregistry}") {
	      dockerImage.push("${env.tag}")
	   }
        }
      }
    }
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi ${env.imagename"}
      }
    }
  }
}