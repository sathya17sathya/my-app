node{
   stage('My GitHUB'){
     git 'https://github.com/sathya17sathya/my-app.git'
   }
   stage('Compile-Package'){

      def mvnHome =  tool name: 'MyMaven', type: 'maven'   
      sh "${mvnHome}/bin/mvn clean package"
	  sh 'mv target/myweb*.war target/newapp.war'
   }
   stage('SonarQube Analysis') {
	        def mvnHome =  tool name: 'MyMaven', type: 'maven'
	        withSonarQubeEnv('sonar') { 
	          sh "${mvnHome}/bin/mvn sonar:sonar"
	        }
	    }
    stage('Build Docker Imager'){
   sh 'docker build -t sathyafriendship/myweb:0.0.2 .'
   }
   stage('Docker Image Push'){
   withCredentials([string(credentialsId: 'dockerPass', variable: 'dockerPassword')]) {
   sh "docker login -u sathyafriendship -p ${dockerPassword}"
    }
   sh 'docker push sathyafriendship/myweb:0.0.2'
   }
   stage('Nexus Image Push'){
   sh "docker login -u admin -p admin123 13.213.8.161:5000"
   sh "docker tag sathyafriendship/myweb:0.0.2 13.213.8.161:5000/latest:1.0.0"
   sh 'docker push 13.213.8.161:5000/latest:1.0.0'
   }
   stage('Remove Previous Container'){
	try{
		sh 'docker rm -f tomcattest'
	}catch(error){
		//  do nothing if there is an exception
	}
   stage('Docker deployment'){
   sh 'docker run -d -p 8090:8080 --name tomcattest sathyafriendship/myweb:0.0.2' 
   }
   }
   }
