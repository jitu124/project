pipeline {
    agent any

    environment {
	  dockerImage = ''
	  registry = 'jitu124/tom'
    }
	
    stages {
        stage('Validate') {
            steps {
                echo 'Validating..'
		sh 'mvn compile'
            }
        }
        stage('Unit Test') {
            steps {
                echo 'Testing..'
		sh 'mvn test'
            }
        }
	stage("build & SonarQube analysis") {
            steps {
              withSonarQubeEnv('Sonarqube') {
                sh 'mvn sonar:sonar'
              }
            }
        }
        stage('package') {
            steps {
                echo 'packing....'
		sh 'mvn clean package'
            }
	} 
	stage('deploy') {
	   steps {
	      	nexusArtifactUploader artifacts: [[artifactId: 'WebAppCal', classifier: '', file: 'target/WebAppCal-1.2.05.war', type: 'war']], credentialsId: 'Nexus', groupId: 'com.web.cal', nexusUrl: '3.226.239.132:8081/nexus', nexusVersion: 'nexus2', protocol: 'http', repository: 'releases', version: '1.2.05'   
	   }
	}	
	stage('build') {
	      steps {
		      script {    
		  dockerImage = docker.build registry
	        }
            }
        } 
	post {
          always {
           mail bcc: '', body: 'pipeline failed', cc: 'jitu.pretam@gmail.com', from: '', replyTo: '', subject: '', to: 'projectdevops1@gmail.com'
          }
        }   
    }     
}
