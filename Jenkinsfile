node{
 stage('Git Checkout'){
	git branch: 'dockercicd', url: 'https://github.com/tseringnam/devopsprojects.git'  
 }
 stage('Maven Package'){
	def mvnHome = tool name: 'maven', type: 'maven'
	sh "${mvnHome}/bin/mvn clean package"
 	sh 'mv target/myweb*.war target/myweb.war'
 }
 
 stage('Build Docker Imager'){
   sh 'docker build -t tseringnamgyal/myweb:0.0.1 .'
 }
 
 stage('Push to Docker Hub'){
 
	 withCredentials([string(credentialsId: 'github-pwd', variable: 'dockerHubPwd')]) {
        sh "docker login -u tseringnamgyal -p ${dockerHubPwd}"
     }
	 sh 'docker push tseringnamgyal/myweb:0.0.1'
 }
 stage('Remove Previous Container'){
	try{
		def dockerRm = 'docker rm -f myweb'
		sshagent(['docker-dev']) {
			sh "ssh -o StrictHostKeyChecking=no ec2-user@3.83.43.200 ${dockerRm}"
		}
	}catch(error){
		//  do nothing if there is an exception
	}
 }
 stage('Deploy to Dev Environment'){
   def dockerRun = 'docker run -d -p 8080:8080 --name myweb tseringnamgyal/myweb:0.0.1 '
   sshagent(['docker-dev']) {
    sh "ssh -o StrictHostKeyChecking=no ec2-user@3.83.43.200 ${dockerRun}"
   }

 }

}
