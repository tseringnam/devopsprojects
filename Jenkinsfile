node{
	stage('SCM Checkout'){
		git branch: 'wartomcat', url: 'https://github.com/tseringnam/devopsprojects.git'
	}
	stage('Compile-Package'){
		def mvnHome = tool name: 'maven', type: 'maven'
		sh "${mvnHome}/bin/mvn package"
	}
	stage('Deploy to Tomcat'){
		sshagent(['ec2-user']) {
		sh 'scp -o StrictHostKeyChecking=no target/*.war ec2-user@52.70.178.69:/opt/tomcat9/webapps/'
	}
	}
}
