pipeline{
	agent any

	tools {
		maven 'local maven'
	}
	stages{
		stage('Build'){
			steps {
				sh 'mvn clean package'
			}
			post {
				success {
					echo 'success' >> ~/github/maven-project/webapp/src/main/webapp/index.jsp
					echo 'Now Archiving...'
					archiveArtifacts artifacts: '**/target/*.war'

				}
			}
		}
		stage('Deploy to staging'){
			steps {
				build job: 'deploy-to-staging'

			}
		}

		stage('Deploy to prod'){
			steps{
				timeout(time:5, unit:'DAYS'){
					input message: 'Approve PRODUCTION Deployment?'
				}
				build job: 'deploy-to-prod'
			}
			post{
				success {
					echo 'Code deployed to Production.'
				}
				failure{
					echo 'Deployment failed.'
				}
			}
		}
		
	}
}