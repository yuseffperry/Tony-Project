pipeline {
    agent any
    environment {
	def mvnHome = tool name: 'maven', type: 'maven'
	def sonarqubeScannerHome = tool name: 'SonarQube Scanner', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
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
		    sh '${mvnHome}/bin/mvn clean jacoco:prepare-agent install jacoco:report'
            }
        }
        stage('SonarQube Analysis') {
            steps {
		    echo 'SonarQube...'
		    withSonarQubeEnv('SonarQube') {
		    sh '${sonarqubeScannerHome}/bin/sonar-scanner -e -Dsonar.projectVersion=0.1 -Dsonar.sources=.'
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
