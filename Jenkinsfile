pipeline {
	environment {
		registry = "devaraj1234/microservice-registry"
		registryCredential = 'docker_hub_id'
		dockerImage = ''
	}	
    agent any
    stages {
       
        stage ('Build Application'){
        	steps {
             	sh 'mvn clean package spring-boot:repackage'
             	}
        	}
        
        stage('Publish Tests Results'){
      		steps {
          		echo "Publish junit Tests Results"
		  		junit '**/target/surefire-reports/TEST-*.xml'
		  		archive 'target/*.jar'
        	}
    	}
    	
    	stage('Build Docker Image') {
      		steps{
 				sh 'docker stop account || true && docker rm account || true'
 				sh 'docker image rm devaraj1234/microservice-registry:account || true'
      			sh 'docker build -t devaraj1234/microservice-registry:account .'
      		}
    	}
    	
    	stage('Push Docker Image') {
            steps {
            script {
				docker.withRegistry( '', registryCredential ) {
                	sh 'docker push devaraj1234/microservice-registry:account'
                	sh 'docker image rm devaraj1234/microservice-registry:account || true'
                	}
               	}
              }
           }
           
        stage('Pull & Run Docker Image') {
        steps{
        	sh 'docker stop account || true && docker rm account || true'
        	sh 'docker pull devaraj1234/microservice-registry:account'
        	sh 'docker run -d --name=account --link consul:consul -p 8081:8081 devaraj1234/microservice-registry:account'
        }
        
        }
        
    }
}