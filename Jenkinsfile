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
       
            stage('Building image') {
	      steps{
		script {
		  dockerImage = docker.build registry + ":$BUILD_NUMBER"
		}
	      }
	    }
	    stage('Deploy Image') {
	      steps{
		script {
		  docker.withRegistry( '', registryCredential ) {
		    dockerImage.push()
		  }
		}
	      }
	    }    
	    
	    
            
    }
}
