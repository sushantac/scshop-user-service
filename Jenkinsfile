pipeline {

	 
	
    agent {
        docker {
            image 'maven:3-alpine'
            args '-v /root/.m2:/root/.m2'
        }
    }
    options {
        skipStagesAfterUnstable()
    }
    stages {
       
            stage('Deliver') { 
	       steps {
            	
		    dockerfile {
			filename 'Dockerfile'
			label 'sushantac/user-service'
			additionalBuildArgs  '--build-arg version=0.0.2'
		    }
	    	}
	    }
	    
	    stage('DELPOY') {
     	        steps {
			withCredentials([string(credentialsId: 'dockerHubPassword1', variable: 'dockerHubPassword')]) {
			    sh "docker login -u sushantac -p ${dockerHubPassword}"
			    sh 'docker push sushantac/user-service:0.0.1'
			}
  	        }
	    }
            
    }
}
