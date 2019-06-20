node{
	stage('SCM Checkout'){
		git branch: 'slacknotification', url: 'https://github.com/prabhatpankaj/devopsprojects.git'
	}
	stage('Compile-Package'){
		def mvnHome = tool name: 'maven', type: 'maven'
		sh "${mvnHome}/bin/mvn package"
	}
	stage('Deploy to Tomcat'){
		sshagent(['tomcat-server']) {
		sh 'scp -o StrictHostKeyChecking=no target/*.war ec2-user@18.232.131.59:/opt/tomcat9/webapps/'
		}
	}
	stage('Email Notification'){
	mail bcc: '', body: 'This is body', cc: '', from: 'prabhatiitbhu@gmail.com', replyTo: 'prabhatiitbhu@gmail.com', subject: 'This is Subject', to: 'prabhat@aptence.com'
	}
	stage('Slack Notification'){
	slackSend baseUrl: 'https://hooks.slack.com/services/', channel: '#jenkins', color: '#439FE0', message: 'New Build deployed', teamDomain: 'intelycore03', tokenCredentialId: 'slack-secret'
	}

}
