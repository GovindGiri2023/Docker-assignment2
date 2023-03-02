pipeline{
	agent {
		label{
			label "built-in"
		}
	}
	parameters{
		string(name: "PORT", description: "Please assign one port number to docker container:")
	}
	stages{
		stage("Cleaning workspace"){
			steps{
				cleanWs ()
			}

		}
		stage("Checkout scm"){
			steps{
				checkout scp
			}
		}

		stage("Parallel stages"){
			parallel{
				
				stage("remmoving if containre is running"){
				    agent{
				    	label{
						    label "Slave-1"
					    }
				    }
				    steps{
						sh "sudo docker container rm -f httpd_${GIT_BRANCH} || true"
		   		    }
		      
		
				}	
        		stage("Creating container"){
        		    				    agent{
				    	label{
						    label "Slave-1"
					    }
				    }
        			steps{
							sh "sudo docker run -dp ${PORT}:80 --name httpd_${GIT_BRANCH} httpd"
        			}

            	}
        	}
        }

		stage("Coping index file to container:"){
			steps{
					sh "sudo docker cp $WORKSPACE/index.html httpd_${GIT_BRANCH}:/usr/local/apache2/htdocs/"
			}
		}
	}
}
	
