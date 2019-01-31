pipeline {
    agent any


    tools {
	maven 'maven'
	jdk 'java8'
    }

    stages {
        stage('Build') {
            steps {
		echo 'Building...'
		sh './mvn package'
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
