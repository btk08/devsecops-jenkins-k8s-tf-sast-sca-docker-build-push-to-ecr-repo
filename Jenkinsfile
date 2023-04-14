pipeline {	
	agent any
	tools {
	maven 'Maven_3_5_2'
	}
	stages{
	stage('CompileandRunSonarAnalysis') {
	steps {
	sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=calobug -Dsonar.organization=calobug -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=47910261b9e4e1c98913e7ea1e9c77ddf3632bee'
	}
	}

	stage('RunSCAAnalysisUsingSnyk') {
            steps {		
				withCredentials([string(credentialsId: 'SNYK_TOKEN', variable: 'SNYK_TOKEN')]) {
					sh 'mvn snyk:test -fn'
				}
			}
    }		

	stage('Build') { 
            steps { 
               withDockerRegistry([credentialsId: "dockerlogin", url: ""]) {
                 script{
                 app =  docker.build("asg")
                 }
               }
            }
    }

	stage('Push') {
            steps {
                script{
                    docker.withRegistry('https://384424111657.dkr.ecr.us-east-1.amazonaws.com', 'ecr:us-east-1:aws-credentials') {
                    app.push("latest")
                    }
                }
            }
    	}
	    
  }
}
