pipeline {
  agent any
  tools { 
        maven 'Maven_3_8_7'  
    }
   stages{
 //    stage('CompileandRunSonarAnalysis') {
 //            steps {	
	// 	sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=deepuorg -Dsonar.organization=deepuorg -Dsonar.host.url=https://sonarcloud.io -Dsonar.token=8ea9f40083e0db4ab94fb64a59eb25e3ae4d8dae'
	// 		}
 //    }

	// stage('RunSCAAnalysisUsingSnyk') {
 //            steps {		
	// 			withCredentials([string(credentialsId: 'SNYK_TOKEN', variable: 'SNYK_TOKEN')]) {
	// 				sh 'mvn snyk:test -fn'
	// 			}
	// 		}
 //    }

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
                    docker.withRegistry('https://471112853464.dkr.ecr.ap-south-1.amazonaws.com', 'ecr:us-west-2:aws-credentials') {
                    app.push("latest")
                    }
                }
            }
    	}
	   
	stage('Kubernetes Deployment of ASG Bugg Web Application') {
	   steps {
	      withKubeConfig([credentialsId: 'kubelogin']) {
		  sh('kubectl delete all --all -n devsecops')
		  sh ('kubectl apply -f deployment.yaml --namespace=devsecops')
		}
	      }
   	}

  }
}
