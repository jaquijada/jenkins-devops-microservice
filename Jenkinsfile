// DECLERATIVE
pipeline {
	//agent any
	//agent { docker { image "maven:3.6.3" } }
	agent { docker { image "eclipse/centos_jdk8" } }

	environment {
		dockerHome = tool "docker"
		mavenHome = tool "maven"
		PATH = "$dockerHome/bin:/$mavenHome/bin:$PATH"
	}

	stages {
		stage('Checkout') {
			steps {
				echo "Checkout"
				sh 'mvn --version'
				sh 'docker --version'
				echo "$PATH"
				echo "Build Number.  : $env.BUILD_NUMBER"
				echo "Build Id.      : $env.BUILD_ID"
				echo "Build Tag      : $env.BUILD_TAG"
				echo "Build Job Name : $env.JOB_NAME"
				echo "Build URL.     : $env.BUILD_URL"
			}
		}

		stage('Compile') {
			steps {
				sh "mvn clean compile"
			}
		}

		stage('Test') {
			steps {
				sh "mvn test"
			}
		}

		stage('Integration Test') {
			steps {
				sh "mvn failsafe:integration-test failsave:verify"
			}
		}

		stage('Package') {
			steps {
				sh "mvn package -DskipTests"
			}
		}

		stage('Build Docker Image') {
			steps {
				script {
					dockerImage = docker.build("currency-exchange-devops")
				}
			}
		}

		stage('Push Docker Image') {
			steps {
				script {
					docker.withRegistry("365821728828.dkr.ecr.us-west-2.amazonaws.com", "aws-ecr") {
						dockerImage.push("${env.BUILD_TAG}")
						dockerImage.push("latest")
					}
				}
			}
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