def DOCKER_HUB_USER="dhiraj2029"

def CONTAINER_NAME = "jenkins-pipeline"
def CONTAINER_TAG = "latest"
def HTTP_PORT = "8090"
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
		 stage('image Prune')
		 {
		   imagePrune(CONTAINER_NAME)
		 }
       
	    stage('image build')
		{
		  imageBuild(CONTAINER_NAME,CONTAINER_TAG)
		}
		
		stage('Push to Image')
		{
           withCredentials([usernamePassword(credentialsId: 'dockerHubAccount', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
            pushToImage(CONTAINER_NAME, CONTAINER_TAG, USERNAME, PASSWORD)
        }
		}
    
	
	    stage('run application')
		{
		  runApp(CONTAINER_NAME,CONTAINER_TAG,DOCKER_HUB_USER,HTTP_PORT)
		 }
		 
		
    }	

def imagePrune(conatinerName){
	try{
	     sh "docker image prune -f"
		 sh "dokcer stop $conatinerName"
		}
	catch(error){}
	}
	
def imageBuild(conatinerName,tag){
    try{
	     sh "docker build -t $conatinerName:$tag -t $conatinerName --pull --no-cache ."
		 echo "Image buile complete"
		}
	catch(error){}
	}
    
def pushToImage(conatinerName,tag,dockeruser,password){
    try{
	     sh "docker login -u $dockeruser -p $password"
		 sh "docker tag $conatinerName:$tag $dockeruser/$conatinerName:$tag"
		 sh "docker push $dockeruser/$conatinerName:$tag"
		}
	catch(error){}
	}

def runApp(conatinerName,tag,user,port)
{
   try{
        sh "docker pull $user/$conatinerName:$tag"
        sh "docker run -d --rm -p $port:$port --name $conatinerName $user/$conatinerName:$tag"
        echo "application is running on port - ${port}"
      }

    catch(error){}
}	
	
