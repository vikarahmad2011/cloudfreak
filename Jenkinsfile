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
     stage("Deploy to Sonar") {
            steps{
                withSonarQubeEnv(installationName: 'sonar-scanner', credentialsId: 'sonarQube') {
                    sh "${ tool ("sonar-scanner")}/sonar-scanner -Dsonar.projectKey=petclinic -Dsonar.projectName=hellospringboot -Dsonar.sourceEncoding=UTF-8 -Dsonar.sources=src"
                }
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
                 def customImage = docker.build('vikar11/samplecode', "./docker")
                 docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                 customImage.push("${env.BUILD_NUMBER}")
                 }                     
           }
        }
	  }
      
    }
}
