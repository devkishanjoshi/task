def dockerBuildPush(imageName){
	docker.withRegistry('https://index.docker.io/v1/', 'dockerhub') {
	def app = docker.build( "${imageName}", '.').push()
	}
	sh 'sleep 2'
	sh "docker rmi ${imageName}"
}

def deploymentK8s(commitId,deploymentName,imageName){
	withKubeConfig([credentialsId: 'updatedConfig']) {    	
		sh "cat k8s/deployment.yaml | sed 's/{{COMMIT_ID}}/${commitId}/g' | kubectl apply -f -"
		sh "kubectl annotate deployment ${deploymentName} kubernetes.io/change-cause='${imageName}' --record=false --overwrite=true"
		sh 'kubectl apply -f k8s/service.yaml'
	}
}
def test(){
	echo "Testing Master branch!!"
}

node {
    def commitId 
	def dev_email
    def dev_name

    stage('SCM') {
	    
		cleanWs()
        checkout scm 
        sh 'git rev-parse --short HEAD > .git/commit-id'
        commitId = readFile('.git/commit-id').trim()
		dev_email = sh(script: "git --no-pager show -s --format='%ae'", returnStdout: true).trim()
		dev_name = sh(script: "git --no-pager show -s --format='%an'", returnStdout: true).trim()		
        }

	def deploymentName = "test-deployment"
	def imageName = "devil1211/test:${commitId}"
	def PROJECT_NAME = 'httpbin'

	try{
		stage('Test') {
			test()
		}	
			
// 		stage('Docker Build/Push Cleanup') {
// 			dockerBuildPush("${imageName}")
// 		}

// 		stage('Deployment') {
//             STAGE=env.STAGE_NAME
// 			deploymentK8s("${commitId}", "${deploymentName}", "${imageName}")	
// 		}
	} catch(error){
        sh "echo ${error}"
	}	
}
