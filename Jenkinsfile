pipeline {
  agent any
    tools {
      maven 'maven3'
                 jdk 'JDK11'
    }
    stages {      
        stage('Build maven ') {
            steps { 
                    sh 'pwd'      
                    sh 'mvn  clean install package'
            }
        }
        
        stage('Copy Artifact') {
           steps { 
                   sh 'pwd'
		   sh 'cp -r target/*.jar docker'
           }
        }
         
        stage('Build docker image') {
           steps {
               script {         
                 def customImage = docker.build('initsixcloud/petclinic', "./docker")
                                    
           }
        }
	  }
	stage('Push Docker Image') {
           steps {
                withDockerRegistry([credentialsId: "docker_auth", url: "https://registry.hub.docker.com/"]) {
                    bat "docker push IMAGE_NAME:latest"
                }
            }
        }
    }
}
