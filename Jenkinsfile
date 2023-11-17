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
                 def customImage = docker.build('vikar11/samplecode', "./docker")
                 docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                 customImage.push("${env.BUILD_NUMBER}")
                 }                     
           }
        }
	  }

       mvn sonar:sonar \
       -Dsonar.projectKey=petclinic \
       -Dsonar.host.url=http://51.20.127.14:9000 \
       -Dsonar.login=a4851866bde9ba1d143f34b93597ff8a1abb0138
       stage("Deploy to Sonar") {
            steps{
                withSonarQubeEnv(installationName: 'sonar-scanner', credentialsId: 'sonar-token') {
                    sh "${ tool ("sonar-scanner")}/sonar-scanner -Dsonar.projectKey=hellospringboot -Dsonar.projectName=hellospringboot -Dsonar.sourceEncoding=UTF-8 -Dsonar.sources=src"
                }
            }
        }
    }
}
