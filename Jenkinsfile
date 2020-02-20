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
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
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
	     //steps {
            	
		 //withDockerRegistry([ credentialsId: "dockerHubCredentials", url: "https://registry-1.docker.io/v2/" ]) {
			  // following commands will be executed within logged docker registry
			  //sh 'docker push sushantac/user-service:0.0.1'
		 //}
			
		//sh 'docker run -d -p 8002:8002 --name user-service sushantac/user-service:0.0.1'
		    agent {
			docker { image 'sushantac/user-service:0.0.1' }
		    }
		    steps {
			sh 'java --version'
		    } 
	      //}
        }
            
    }
}
