node(){
	
	def mvnHome = tool 'Maven'
	def sonarScannerHome = tool 'Sonar'
	
	try {
		stage('Checkout Code'){
			checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: '7fc6f676-451f-4e16-b52d-02049fa25487', url: 'https://github.com/GauravBarua/SonarQubeCoverageJava.git']])
		}
		
		stage('Maven Build'){
			sh "${mvnHome}/bin/mvn clean install"
		}
		
		stage('Test Cases Execution'){
			sh "${mvnHome}/bin/mvn test"
		}
		
		stage('SonarQube Analysis'){
			withSonarQubeEnv(credentialsId: 'd43f2f50-7ce8-44b8-a97a-14981ea5b394') {
    			sh "${sonarScannerHome}/bin/sonar-scanner -Dsonar.host.url=http://http://20.198.113.109:9000// -Dsonar.login=${SONARQUBE_TOKEN} -Dsonar.projectKey=com.example:java-example-project"
                        }
		}
		
	}
	catch (Exception e){
		currentBuild.result = 'FAILURE'
		echo currentBuild.currentResult
	}finally{
		emailext attachLog: true, attachmentsPattern: 'target/surefire-reports/*.xml', 
			 body: '''$PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS:
	Check console output at $BUILD_URL to view the results.''', 
			compressLog: true, recipientProviders: [buildUser(), requestor()], subject: '$PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS!', to: 'gauravbarua008@gmail.com'
	}
}
