node {
   echo 'Build and Deployment started'
   def service_name = params.service_name
   def username = params.username
   def password = params.password
   def credentialid = params.credentialid
   def cloud_name = params.cloud_name
   def buser = params.buser
   def bpass = params.bpass
   def borg = params.borg
   def bspace = params.bspace
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
	
	stage ('Deploy service'){
    withEnv(["PATH=${workspace}/cf-cli:$PATH"])  {
        sh 'cf api https://api.ng.bluemix.net'
        sh 'cf login -u ' + buser + ' - p '+ bpass +' -o ' + borg + ' -s '+ bspace
		dir(service_name){
        if(cloud_name == "bluemix"){
        
        sh 'cf push'
        }
	}
     }   
	}
	
	
	
	
	

}
