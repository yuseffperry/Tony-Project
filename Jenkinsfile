pipeline {
    agent any
    environment {
	def sonarqubeScannerHome = tool name: 'SonarQube Scanner', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
    }


    stages {
        stage('Build') {
            steps {
		    echo 'Building...'
		    sh './gradlew clean assemble'
            }
        }
        stage('Test') {
            steps {
		    echo 'Testing...'
		    sh './gradlew clean test'
		    junit allowEmptyResults: true, testResults: 'target/test-results/test/*.xml'
		    publishHTML([allowMissing: true,
		      alwaysLinkToLastBuild: true,
		      keepAll: false,
		      reportDir: 'target/reports/tests/test',
		      reportFiles: 'index.html',
		      reportName: 'Jacoco Exmaple Gradle Test Results',
		      reportTitles: ''
		        ])
            }
        }
        stage('SonarQube Analysis') {
            steps {
		    echo 'SonarQube...'
		    withSonarQubeEnv('SonarQube') {
		    sh '${sonarqubeScannerHome}/bin/sonar-scanner -Dsonar.projectVersion=0.1'
		  }
            }
        }
        stage('Publish to Nexus') {
            steps {
                withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'jacocoexample-nexus-upload', usernameVariable: 'NEXUS_CREDENTIALS_USR', passwordVariable: 'NEXUS_CREDENTIALS_PSW']]) {
		    echo 'Nexus...'
            sh './gradlew publish'
                }
            }
        }
    }
}
