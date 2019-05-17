def DOCKER_HUB_USER="dhiraj2029"
def HTTP_PORT="8090"

node{
       stage('initilize')
	   {
	      def dockerHome = tool 'myDocker'
		  def mavenHome =  tool 'myMaven'
		  env.PATH = "${dockerHome}/bin:${mavenHome}/bin:${env.PATH}"
	   }
		
		stage('checkout')
		{
		  checkout scm
		}
		
		stage('Build')
		{
		  sh "mvn clean install"
        }
     
        stage('sonar')
        {
          try{
		       sh "mvn sonar:sonar"
			 }
		  catch(error)
		  {
		    echo "sonar servers could not reached ${error}"
          }
        }

    }		
		  
