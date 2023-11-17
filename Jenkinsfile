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
       stage('sonar-scanner') {
	  def sonarqubeScannerHome = tool name: 'sonar', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
	  withCredentials([string(credentialsId: 'sonar', variable: 'sonarLogin')]) {
	    sh "${sonarquberScannerHome}/bin/sonar-scanner -e -Dsonar,host.url=http://51.20.127.14:9000/ -Dsonar.login=${sonarLogin} -Dsonar.projectName=gs.gradle -Dsonar.projectVersion=${env.BUILD_NUMBER} -Dsonar.projectKey=GS -Dsonar.sources=complete/src/main/ -Dsonar.test=complete/src/test/ -Dsonar.language=java"
	  }
	}
    }
}
