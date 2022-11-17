pipeline {
	agent any
	stages {
	        stage('Checkout SCM') {
            steps {
                git branch:'main', url:'https://github.com/tanchiawei/cw-repo.git'
            }
        }
		

		stage('OWASP DependencyCheck') {
			steps {
				dependencyCheck additionalArguments: '--format HTML --format XML --suppression suppression.xml', odcInstallation: 'OWASP-Dependency-Check'
			}
		}
		        stage('Code Quality Check via SonarQube') {
            steps {
                script {
                    def scannerHome = tool 'SonarQube'
                    withSonarQubeEnv('SonarQube') {
                        sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=OWASP -Dsonar.sources=.  -Dsonar.host.url=http://localhost:9000 -Dsonar.login=sqp_0ab068194669b8ca38b7782c3e5cc45c3daf2bd2"
                    }
                }
            }
        }
	}	
	post {
		success {
			dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
		}
	}
}
