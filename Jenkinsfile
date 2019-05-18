def DOCKER_HUB_USER="dhiraj2029"
def HTTP_PORT="8090"
def CONTAINER_NAME = "jenkins-pipeline"
def CONTAINER_TAG = "latest"

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
		 stage("image Prune")
		 {
		   imagePrune(CONTAINER_NAME)
		 }
       
	    stage("image build")
		{
		  imageBuild(CONTAINER_NAME,CONTAINER_TAG)
		}
		 
    }	

def imagePrune(conatinerName){
	try{
	     sh "docker image prune -f"
		 sh "dokcer stop image $conatinerName"
		}
	catch(error){}
	}
	
def ImageBuild(conatinerName,tag){
    try{
	     sh "docker build -t $conatinerName:$tag -t $conatinerName --pull --no-cache ."
		 echo "Image buile complete"
		}
	catch(error){}
	}
    
