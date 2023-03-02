pipeline{
	agent {
		label{
			label "built-in"
		}
	}
	
	stages{
		stage("Cleaning workspace"){
			steps{
				cleanWs ()
			}

		}
		stage("Checkout scm"){
			steps{
				checkout scm
			}
		}

		stage("Parallel stages"){
			parallel{
				
				stage("remmoving if containre is running"){
				    agent{
				    	label{
						    label "slave-1"
					    }
				    }
				    steps{
						sh "sudo docker container rm -f httpd_${GIT_BRANCH} || true"
		   		    }
		      
		
				}	
        		stage("Creating container"){
        		    				    agent{
				    	label{
						    label "slave-1"
					    }
				    }
        			steps{
							sh "sudo docker run -dp ${PORT}:80 --name httpd_${GIT_BRANCH} httpd"
        			}

            	}
        	}
        }

		steps{
				sshagent(credentials : ["ad5d0717-3dfb-469a-8d83-26686a49abee"]) {
				
					sh'''
					sftp  -o StrictHostKeyChecking=no ec2-user@172.31.81.49 <<EOF
					put  $WORKSPACE/gameoflife-web/target/gameoflife.war /opt/
					exit
		}
	}
}
	
