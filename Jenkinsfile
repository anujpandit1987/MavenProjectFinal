node('master') {
  ansiColor('xterm') {
	stage ('checkout code'){
		checkout scm
	}
	
	stage ('Build'){
		sh "mvn clean install -Dmaven.test.skip=true"
	}

	stage ('Test Cases Execution'){
		sh "mvn clean org.jacoco:jacoco-maven-plugin:prepare-agent install -Pcoverage-per-test"
	}

	stage("build & SonarQube analysis") {
          
               sh 'mvn clean package sonar:sonar'
              
        }

	stage ('Archive Artifacts'){
		archiveArtifacts artifacts: 'target/*.war'
	}
	
	stage ('Deployment'){
		/*ansiblePlaybook( 
        		playbook: 'deploy.yml',
        		inventory: '/etc/ansible/hosts', 
			extras: '--become',
        		colorized: true) */
	}
	stage ('Notification'){
		//slackSend color: 'good', message: 'Deployment Sucessful'
		emailext (
		      subject: "Job Completed",
		      body: "Jenkins Pipeline Job for Maven Build got completed !!!",
		      to: "anujpandit1987@gmail.com"
		    )
	}
   }
}
