pipeline {
    agent any


    stages {
        stage('Build') {
            steps {
		def mvnHome = tool name: 'maven', type: 'maven'
		sh '${mvnHome}/bin/mvn package'
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
