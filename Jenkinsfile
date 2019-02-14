pipeline {
    agent any


    stages {
        stage('Ansible Installation') {
            steps {
            echo 'Installing...'
            //Install MongoDB, Jira, ELK

            sh 'ansible-galaxy install -p roles -r requirements -f'

            ansiColor('xterm') {
                ansiblePlaybook (
                    playbook: 'playbook.yml',
                    inventory: 'inventory.ini',
                    installation: 'ansible',
                    credentialsId: '2869f3eb-e1ed-4a1e-9c52-9bc4f7b3c8dc',
                    colorized: true
                    )
                }
            }
        }
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
        stage('Publish Snapshot to Artifactory') {
            steps {
                withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'jacocoexample-artifactory-upload', usernameVariable: 'ARTIFACTORY_CREDENTIALS_USR', passwordVariable: 'ARTIFACTORY_CREDENTIALS_PSW']]) {
		    echo 'Artifactory...'
            sh './gradlew publish'
                }
            }
        }
    }
}
