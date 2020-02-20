//This is a script for local windows machine. Need to create a better declarative pipeline script for linux on aws

node{
    
    stage('MVN Package - user-service'){
        def mvnHome = tool name: 'Apache Maven', type: 'maven'
        def mvnCMD = "${mvnHome}/bin/mvn"
        
        sh label: '', script: "\"${mvnCMD}\" clean package"
    }
    
    stage('Build Docker Image - user-service') {
        sh label: '', script: 'docker build -t sushantac/user-service:0.0.1 --file Dockerfile .'
    }
    
    stage('Push to docker hub - user-service') {
	
	    withCredentials([string(credentialsId: 'dockerHubPassword1', variable: 'dockerHubPassword')]) {
            sh label: '', script: "docker login -u sushantac -p ${dockerHubPassword}"
        }

        sh label: '', script: 'docker push sushantac/user-service:0.0.1'
    }

    stage('Run container on Dev server'){
        try{
            sh label: '', script: 'docker stop user-service'
        } catch(all) {

        }
        
        try{
            sh label: '', script: 'docker rm user-service'
        } catch(all) {

        }

        sh label: '', script: 'docker run -d -p 8002:8002 --name user-service sushantac/user-service:0.0.1'
    }
    
}
