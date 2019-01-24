pipeline {
	agent {
		docker {
			image 'maven:3.6-alpine' 
			args '-v maven-repo:/root/.m2'
		}
	}
	
	stages {
		stage('Build') { 
			steps {
				sh 'mvn -B -DskipTests clean package' 
			}
		}
		
		stage('Test') {
			steps {
				sh 'mvn test'
			}
			post {
				always {
					junit 'target/surefire-reports/*.xml'
				}
			}
		}
		
		stage('Scan with LifeCycle') {
			steps {
				nexusPolicyEvaluation failBuildOnNetworkError: false, iqApplication: "sample-app", 
				iqScanPatterns: [[scanPattern: ""]], iqStage: "build", jobCredentialsId: ''
			}
		}
		
		stage('Deliver') { 
			steps {
				sh './jenkins/scripts/deliver.sh' 
			}
		}
	}
}