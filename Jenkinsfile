node('master') {
	stage ('checkout code'){
		git 'https://github.com/AmolVarwade/CICD_Project/'
	}
	stage ('checkout code'){
		checkout scm
	}
	
	stage ('Build'){
		sh "mvn clean install -Dmaven.test.skip=true"
	}

	stage ('Test Cases Execution'){
		sh "mvn clean org.jacoco:jacoco-maven-plugin:prepare-agent install -Pcoverage-per-test"
	}

	stage ('Sonar Analysis'){
		sh 'mvn sonar:sonar -Dsonar.host.url=http://52.90.114.217:9000 -Dsonar.login=3f2d512066c968a048575e6a0001e6bcffe61601'
	}

	stage ('Archive Artifacts'){
		archiveArtifacts artifacts: 'target/*.war'
	}
	
	stage ('Deployment'){
		deploy adapters: [tomcat8(credentialsId: 'tomcatcred', path: '', url: 'http://localhost:80')], contextPath: 'simplilearn', war: 'target/*.war'
	}
	stage ('Notification'){
		//slackSend color: 'good', message: 'Deployment Sucessful'
		emailext (
		      subject: "Job Completed",
		      body: "Jenkins Pipeline Job for Maven Build got completed !!!",
		      to: "amolv105@gmail.com"
		    )
	}
}
