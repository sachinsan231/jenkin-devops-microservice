// node {
// 	stage('Build') {
// 		echo "Build"
// 	}
// 	stage('Test') {
// 		echo "Test"
// 	}
// }

pipeline{
	agent any
	environment{
		dockerHome = tool 'myDocker'
		mavenHome = tool 'myMaven'
		PATH = "$dockerHome/bin:$mavenHome/bin:$PATH"
	}
	stages{
		stage('Checkout'){
			steps{
				sh 'mvn --version'
				sh 'docker version'
				echo "Build"
			}
		}
		stage('Compile'){
			steps{
				sh 'mvn clean compile'
			}
		}
		stage('Test'){
			steps{
				sh "mvn test"
			}
		}
		stage('Integration Test'){
			steps{
				sh 'mvn failsafe:integration-test'
			}
		}

		stage('Build Docker Image'){
			steps{
				//docker build -t sachinsan2131/currency-exchange-devops:${env:BUILD_TAG}
				script{
					dockerImage = docker.build("sachinsan2131/currency-exchange-devops:${env:BUILD_TAG}")
				}
			}
		}

		stage('Push Docker Image'){
			script{
					dockerImage.push('latest');
				}
		}
	}
	
}