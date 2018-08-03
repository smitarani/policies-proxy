pipeline {
 	agent any

 	stages {
 	    stage('Clean') {
 	        steps {
				    
  
 	            bat "mvn clean"
 	       			}
 	    	

		}
 	 
 	    stage('Build proxy bundle') {
 	        steps {
				  

					   withCredentials([usernamePassword(credentialsId: "edge-cred",
                                passwordVariable: 'apigee_pwd',
                                usernameVariable: 'apigee_user')]) {
 	
 	            bat "mvn package -Ptest -Denv=${params.apigee_env} -Dorg=${params.apigee_org}"+
                                    " -Dusername=${apigee_user} -Dpassword=${apigee_pwd}"
 	 
 	
            	  
 	    	}

		}

		 }

		stage('Pre-Deployment Configuration ') {
            steps {
                

                    println "Predeployment of Caches "
                    withCredentials([usernamePassword(credentialsId: "edge-cred",
                            passwordVariable: 'apigee_pwd',
                            usernameVariable: 'apigee_user')]) {
                        script {
                            if (fileExists("resources/edge/env/${params.apigee_env}/caches.json")) {
                                bat "mvn apigee-config:caches " +
                                        "    -Ptest -Denv=${params.apigee_env} -Dorg=${params.apigee_org} " +
                                        "    -Dusername=${apigee_user} " +
                                        "    -Dpassword=${apigee_pwd}"
                            }


                        if (fileExists("resources/edge/env/${params.apigee_env}/kvms.json")) {
                            bat "mvn apigee-config:keyvaluemaps " +
                                    "    -Ptest -Denv=${params.apigee_env} -Dorg=${params.apigee_org} " +
                                    "    -Dusername=${apigee_user} " +
                                    "    -Dpassword=${apigee_pwd}"
                        }

                        if (fileExists("resources/edge/env/${params.apigee_env}/targetServers.json")) {
                            bat "mvn apigee-config:targetservers " +
                                    "    -Ptest -Denv=${params.apigee_env} -Dorg=${params.apigee_org} " +
                                    "    -Dusername=${apigee_user} " +
                                    "    -Dpassword=${apigee_pwd}"
                        }
                    }
                }
                
            }
        }
		 stage('Deploy proxy bundle') {
                steps {
                    
                        withCredentials([usernamePassword(credentialsId: "edge-cred",
                                passwordVariable: 'apigee_pwd',
                                usernameVariable: 'apigee_user')]) {
                    bat "mvn install -Ptest -Denv=${params.apigee_env} -Dorg=${params.apigee_org} " +
                                    " -Dusername=${apigee_user} -Dpassword=${apigee_pwd}"
                        }
                    }
                
            }
 	 
 	}
}