node {
    def app

    stage('Clone repository') {
        /* Cloning the Repository to our Workspace */

        checkout scm
    }

    stage('Build image') {
        /* This builds the actual image */
	    
	    app = docker.build("sampleacc54/nodeapp1")
    }

    stage('Test image') {
        
        app.inside {
            echo "Tests passed"
        }
    }

    stage('Push image') {
        /* 
			You would need to first register with DockerHub before you can push images to your account
		*/
	    //docker.withRegistry(credentialsId: 'dockerHub', url: 'https://registry.hub.docker.com')
        docker.withRegistry('https://registry.hub.docker.com', 'dockerHub')
	    {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
            } 
                echo "Trying to Push Docker Build to DockerHub"
    }
	stage('Run container on DevServer'){
		sshagent(['citeam']) {
			sh label: '', script: 'ssh -oStrictHostKeyChecking=no -y citeam@194.110.231.139 uptime'
			sh label: '', script: 'ssh -v citeam@194.110.231.139 \' docker run -p 8000:8000 -d --name nodeapp sampleacc54/nodeapp1\''
}	
} 
/* stage('DeployToProduction') {
	sshagent(['citeam']) {
	sh 'ssh -oStrictHostKeyChecking=no -y -v citeam@194.110.231.139' 
		sh 'kubernetesDeploy configs: 'kube.yml', kubeconfigId: 'kubeconfig''
      }      
        } */
    }	
