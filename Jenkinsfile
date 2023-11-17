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
       stage(SonarQube Analysis'){
	  def scannerHome  tool 'SonarQube'
	  withSonarQubeEnv{'SonarQube'}{
	  sh 
	***/var/lib/jenkins/tools/hudson.plugins.sonar.SonarRunnerInstallation/SonarQube/bin/sonar-scanner\
	 -D sonar.projectVersion=1.0-SNAPSHOT\
	  -D sonar.login=admin\
	  -D sonar.password=admin\
	  -D sonar.projectBaseDir/Var/lib/jenkins/workspace/jenkins-sonar/\
	 -D sonar.projectKey=petclinic\
	 -D sonar.sourceEncoding=UTF-8\
	 -D sonar.language=java \
	 -D sonar.source=my-app/src/main\
	 -D sonar.tests=my.app/src/test\
	 -D sonar.host.url=http://51.20.127.14:9000/***
	 }
	 }
	 }
    }
}
