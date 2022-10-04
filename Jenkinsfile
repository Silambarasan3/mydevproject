node{
   stage('SCM Checkout'){
     git 'https://github.com/Silambarasan3/mydevproject.git'
   }

stage('Compile-Package'){

      def mvnHome =  tool name: 'mymaven', type: 'maven'   
      sh "${mvnHome}/bin/mvn clean package"
	  sh 'mv target/myweb*.war target/newapp.war'
   }
   
stage('SonarQube Analysis') {
	        def mvnHome =  tool name: 'mymaven', type: 'maven'
	        withSonarQubeEnv('sonar') { 
	          sh "${mvnHome}/bin/mvn sonar:sonar"
	        }
	    }
 stage('Build Docker Imager'){
   sh 'docker build -t silambarasan3r/myapp:0.0.2 .'
   }
   stage('Docker Image Push'){
   withCredentials([string(credentialsId: 'dockerPass', variable: 'dockerPassword')]) {
   sh "docker login -u silambarasan3r -p ${dockerPassword}"
    }
   sh 'docker push silambarasan3r/myapp:0.0.2'
   }
    stage('Nexus Image Push'){

withCredentials([string(credentialsId: 'nexusPass', variable: 'nexusPassword')]) {
    sh "docker login -u admin -p ${nexusPassword} 52.91.242.133:8083"

   sh "docker tag silambarasan3r/myapp:0.0.2 52.91.242.133:8083/simbu:1.0.0"
   sh 'docker push 52.91.242.133:8083/simbu:1.0.0'
   }
}
   stage('Remove Previous Container'){
	try{
		sh 'docker rm -f myserver'
	}catch(error){
		//  do nothing if there is an exception
	}
   stage('Docker deployment'){
   sh 'docker run -d -p 8090:8080 --name myserver silambarasan3r/myapp:0.0.2' 
   }
}
}
