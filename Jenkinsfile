#!groovy
node {
   echo 'Build and Deployment started'
   def service_name = params.service_name
   def username = params.username
   def password = params.password
   def credentialid = params.credentialid
   def cloud_name = params.cloud_name
   def puser = params.puser
   def ppass = params.ppass
   def mvn_version = 'Maven 3.3.9'
   def workspace = pwd()
   def repo_protocol				= "https://"
   def var_github_repo = repo_protocol + "github.com/anshu2185" + "/"
	 git_repo = repo_protocol + "api.github.com/user/repos"
   def service_template
   echo "Starting build and deployment of the service.."
	//echo "params : $params"
	
	stage('Get Service')
	{
		try{
			sh 'rm -rf ' + service_name + '*'
		}
		catch(error){
			//do nothing
		}

		try{
			sh 'mkdir ' + service_name
			dir(service_name)
			{
				checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: credentialid, url: var_github_repo + service_name + '.git']]])
			}
            dir('cf-cli') {
            sh 'wget "https://cli.run.pivotal.io/stable?release=linux64-binary&source=github" -O cf-cli.tgz'
            sh 'tar -zxvf cf-cli.tgz'
        }
		}
		catch(error){
			//do nothing
		}

	}
    
    stage('Build service'){
	  withEnv( ["PATH+MAVEN=${tool mvn_version}/bin"] ) {  
              dir(service_name){
              sh 'mvn clean install'
    }
	  }
    }
	
	stage ('Deploy service'){
    withEnv(["PATH=${workspace}/cf-cli:$PATH"])  {
	    if(cloud_name == "pcf"){
        sh 'cf login -a https://api.system.prokarma.com -u $puser -p $ppass --skip-ssl-validation'
		    dir(service_name){
        
         sh 'cf push'
       
	}
        }
        
     }   
	}
	
	
	
	
	

}
