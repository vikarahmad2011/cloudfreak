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
         
       stage('Build Docker Image'){
	   steps {
	       script {
		 sh 'docker build -t :latest .'
		}
	     }
	 }
	stage('Deploy Docker Image') {
	    steps {
		script {
		withCredentials([string(credentialsId: 'dockerhub', variable: 'dockerhub')]){
		   sh 'docker login -u vikar11 -p $(dockerhub)'
		}
	    }
	}
          }	
    }
}
