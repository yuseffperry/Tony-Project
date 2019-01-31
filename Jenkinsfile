pipeline {
    agent any
    environment {
	def mvnHome = tool name: 'maven', type: 'maven'
    }


    stages {
        stage('Build') {
            steps {
		echo 'Building...'
		//sh '${mvnHome}/bin/mvn package'
		bat "cd ${mvnHome}\\bin && mvn package"
            }
        }
        stage('Test') {
            steps {
		echo 'Testing...'
                sh './jenkins_build.sh'
                junit '*/build/test-results/*.xml'
                step( [ $class: 'JacocoPublisher' ] )
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying...'
            }
        }
    }
}
