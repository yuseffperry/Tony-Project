pipeline {
    agent any
    environment {
	def mvnHome = tool name: 'maven', type: 'maven'
    }


    stages {
        stage('Build') {
            steps {
		    echo 'Building...'
		    sh '${mvnHome}/bin/mvn install'
            }
        }
        stage('Test') {
            steps {
		    echo 'Testing...'
            }
        }
        stage('SonarQube Analysis') {
            steps {
		    echo 'SonarQube...'
		    withSonarQubeEnv('SonarQube') {
		    sh '${mvnHome}/bin/mvn sonarqube -Dsonar.projectVersion=0.1'
		     }
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying...'
            }
        }
    }
}
