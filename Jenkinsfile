pipeline {

	environment {
		dockerImage = ''
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
            	
		    //dockerfile {
			//filename 'Dockerfile'
			//label 'sushantac/user-service'
			//additionalBuildArgs  '--build-arg version=0.0.2'
		    //}
			
			script {
			  dockerImage = docker.build 'sushantac/user-service' + ":$BUILD_NUMBER"
			}
	
	     }
        }
        
	stage('Deploy') { 
	     steps {
            	
		 //withDockerRegistry([ credentialsId: "dockerHubCredentials", url: "https://registry-1.docker.io/v2/" ]) {
			  // following commands will be executed within logged docker registry
			  //sh 'docker push sushantac/user-service:0.0.1'
		 //}
			script {
			  docker.withRegistry( '', "dockerHubCredentials" ) {
				dockerImage.push()
			  }
			}
		  
	     }
        }
            
    }
}
