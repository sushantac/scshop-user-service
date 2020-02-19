pipeline{
  agent any
  stages {
    
    stage('MVN Package - user-service'){
		steps{
			sh "mvn clean package"
		}
    }
        
    stage('Build Docker Image - user-service') {
		steps{
			sh 'docker build -t sushantac/user-service:0.0.1 --file user-service/Dockerfile .'
		}
    }
    
    stage('Push to docker hub - user-service') {
		steps{
		  withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'password', usernameVariable: 'username')]) {
				sh "docker login -u sushantac -p ${dockerHubPassword}"
		  }

		  sh 'docker push sushantac/user-service:0.0.1'
		}
    }
    
  }
}
