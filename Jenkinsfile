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
         
       node {

         checkout scm
         def dockerImage

         stage('Build image') {
           dockerImage = docker.build("initsixcloud/petclinic:latest")
         }

         stage('Push image') {
           docker.withRegistry('https://registry-1.docker.io/v2/', 'dockerhub') {
             dockerImage.push()
           }
         }

       }	
    }
}
