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
		
		stage('Deliver') { 
			steps {
				sh './jenkins/scripts/deliver.sh' 
			}
		}

		stage('Nexus Lifecycle Analysis') {
			postGitHub commitId, 'pending', 'analysis', 'Nexus Lifecycle Analysis is running'

			try {
				def policyEvaluation = nexusPolicyEvaluation iqApplication: 'sample-app', iqStage: 'build'
				postGitHub commitId, 'success', 'analysis', 'Nexus Lifecycle Analysis succeeded', "${policyEvaluation.applicationCompositionReportUrl}"
			} catch (error) {
				def policyEvaluation = error.policyEvaluation
				postGitHub commitId, 'failure', 'analysis', 'Nexus Lifecycle Analysis failed', "${policyEvaluation.applicationCompositionReportUrl}"
				throw error
			}
		}
	}
}