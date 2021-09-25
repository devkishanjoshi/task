node {
    def commitId 
	def dev_email

    stage('SCM') {
		cleanWs()
        checkout scm 
        sh 'git rev-parse --short HEAD > .git/commit-id'
        commitId = readFile('.git/commit-id').trim()
		dev_email = sh(script: "git --no-pager show -s --format='%ae'", returnStdout: true).trim()
		dev_name = sh(script: "git --no-pager show -s --format='%an'", returnStdout: true).trim()		
        }



	if (env.BRANCH_NAME.equals("test-jenkins")){


		try{	
			stage('Docker Build/Push Cleanup') {
                STAGE=env.STAGE_NAME
			sh "echo branch test-jenkins"
			}

		} catch(error){
			sh "echo Error occured"
		}	 
	}	


}
