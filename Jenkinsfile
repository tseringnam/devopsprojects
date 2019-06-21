node{
	stage('SCM Checkout'){
		git branch: 'slacknotification', url: 'https://github.com/prabhatpankaj/devopsprojects.git'
	}
	stage('Compile-Package'){
		def mvnHome = tool name: 'maven', type: 'maven'
		sh "${mvnHome}/bin/mvn package"
	}
	stage('Deploy to Tomcat'){
		sshagent(['ec2-user']) {
		sh 'scp -o StrictHostKeyChecking=no target/*.war ec2-user@3.83.13.53:/opt/tomcat9/webapps/'
		}
	}
	stage('Email Notification'){
	mail bcc: '', body: 'This is body', cc: '', from: 'grgtsering@gmail.com', replyTo: 'grgtsering@gmail.com', subject: 'This is Subject', to: 'prabhat@aptence.com'
	}
	
	stage('Slack Notification'){
	slackSend baseUrl: 'https://hooks.slack.com/services/', channel: '#jenkins', color: '#439FE0', message: 'New Build deployed', teamDomain: 'intelycore4', tokenCredentialId: 'slack-secret'
	}
      
}
