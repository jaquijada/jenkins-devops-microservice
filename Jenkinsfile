// DECLERATIVE
pipeline {
	agent any
	stages {
		stage('Build') {
			steps {
				echo "Build"
			}
		}
		stage('Test') {
			steps {
				echo "Test"
			}
		}
		stage('Integration Test') {
			steps {
				echo "Integration Test"
			}
		}

		post {
			always {
				echo "I always run"
			}
			success {
				echo "I run on success"
			}
			failure {
				echo "I run on failure"
			}
			changed {
				echo "I run on changed"
			}
		}
	} 
}