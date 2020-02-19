pipeline{
  agent any
  stages {
    
    stage('MVN Package - user-service'){
        def mvnHome = tool name: 'Apache Maven', type: 'maven'
        def mvnCMD = "${mvnHome}/bin/mvn"

        sh label: '', script: "\"${mvnCMD}\" clean package"
    }
        
    stage('Build Docker Image - user-service') {
      
        sh label: '', script: 'docker build -t sushantac/user-service:0.0.1 --file user-service/Dockerfile .'
    }
    
    stage('Push to docker hub - user-service') {
  	
      withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'password', usernameVariable: 'username')]) {
            sh label: '', script: "docker login -u sushantac -p ${dockerHubPassword}"
      }

      sh label: '', script: 'docker push sushantac/user-service:0.0.1'
    }
    
  }
}
