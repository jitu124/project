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
              withSonarQubeEnv('SonarQube') {
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
	stage('build') {
	      steps {
		      script {    
		  dockerImage = docker.build registry
	        }
            }
        } 
    }     
}
