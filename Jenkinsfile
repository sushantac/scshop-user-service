pipeline {

	  environment {
	    registry = "sushantac/user-service"
	    registryCredential = "dockerHubCredentials"
	  }
	
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

		stage('Deploy') { 
		     steps {

			 withDockerRegistry([ credentialsId: "dockerHubCredentials", url: "https://registry-1.docker.io/v2/" ]) {
				  // following commands will be executed within logged docker registry
				  sh 'docker push sushantac/user-service:0.0.1'
			 }


		     }
		}
	    
	    
	    stage {
	     	withCredentials([string(credentialsId: 'dockerHubCredentials', variable: 'dockerHubPassword')]) {
		    sh "docker login -u sushantac -p ${dockerHubPassword}"
		    sh 'docker push sushantac/user-service:0.0.1'
		}

		
	    }
	    
	    
            
    }
}
