pipeline {
  agent any
  tools { 
        maven 'Maven_3_2_5'  
    }
   stages{
    stage('CompileandRunSonarAnalysis') {
       steps {	
		sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=alamdeen-project -Dsonar.organization=alamdeen-organisation -Dsonar.host.url=https://sonarcloud.io -Dsonar.token=79045dce998106f6a0243eb862f24277991503a9'
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
                    docker.withRegistry('https://941377136224.dkr.ecr.us-west-2.amazonaws.com', 'ecr:us-west-2:aws-credentials') {
                    app.push("latest")
                    }
                }
            }
    	}


  }
}
