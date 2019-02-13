pipeline {
    agent any


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
		    sh './gradlew test'
		    junit allowEmptyResults: true, testResults: 'build/test-results/test/*.xml'
		    publishHTML([allowMissing: true,
		      alwaysLinkToLastBuild: true,
		      keepAll: false,
		      reportDir: 'build/reports/tests/test',
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
		    sh './gradlew sonarqube -Dsonar.projectVersion=0.1'
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
