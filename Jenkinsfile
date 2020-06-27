pipeline {
   agent any

	stages {
		stage('Complile & Package') {
			steps {
				// Get some code from a GitHub repository
				git 'https://github.com/kmayer10/liquor-shop-demo.git'
	
				// Run Maven on a Unix agent.
				//sh "mvn -Dmaven.test.failure.ignore=true clean package"
	
				// To run Maven on a Windows agent, use
				bat "mvn -Dmaven.test.failure.ignore=true clean package"
			}
	
			post {
				// If Maven was able to run the tests, even if some of the test
				// failed, record the test results and archive the jar file.
				success {
					// junit '**/target/surefire-reports/TEST-*.xml'
					archiveArtifacts 'target/*.war'
				}
			}
		}
		stage('Static Scan'){
			parallel{
				stage('Sonar Scan'){
					steps{
						git 'https://github.com/kmayer10/liquor-shop-demo.git'
						bat "mvn clean test sonar:sonar"
					}
				}
				stage('OWASP DC Scan'){
					steps{
						git 'https://github.com/kmayer10/liquor-shop-demo.git'
						bat "mvn clean install"
					}
				}
			}
		}
	}
}