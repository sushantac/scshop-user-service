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
                sh 'docker build -t sushantac/user-service:0.0.1 --file Dockerfile .'
            }
        }
        
         stage('Deploy') { 
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'password', usernameVariable: 'username')]) {
				  sh "docker login -u sushantac -p ${dockerHubPassword}"
		}
           
                sh 'docker push sushantac/user-service:0.0.1' 
            }
         }
        
    }
}
