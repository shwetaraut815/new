pipeline{
     agent{
	   label{
	        label "built-in"
			customWorkspace "/project/target/"
		}

	 }
	
    tools {
        maven 'Maven3'   // 👈 Configure this in Jenkins Tools before running
    }
	 
	 stages{
	    stage ("git pull") {
		    steps {
			  git branch: "master", url: "https://github.com/shwetaraut815/project.git"
			  
			}
		}
	    stage("build"){
		    steps {
			   sh "mvn clean install"
			 
			}
		}
		stage ("upload s3"){
		    steps {
				
			   sh "aws s3 mb s3://war-deploy-3573"
			   sh "sudo aws s3 cp target/LoginWebApp.war s3://war-deploy-3573"
			}
		}
		 stage ("get war"){
		     agent {
			    label{
				     label "slave-1"
				 }
			 }
			  steps {
			    dir ("/mnt/server/apache-tomcat-10.1.46/webapps"){
			    sh "sudo aws s3 cp s3://war-deploy-3573/LoginWebApp.war ."
				}
			  } 
			 stage('Clean Old WAR') {
            steps {
                sh "rm -f ${TOMCAT_PATH}/LoginWebApp.war"
            }
        }
		 }
		 
	     
	 }
}
