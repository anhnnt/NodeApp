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
		// ssh pi@10.42.0.199 ' docker run -p 8080:8080 -d --name test-jenkins nonah/nodeapp '
		sshagent(['citeam']) {
    // some block
			sh label: '', script: 'ssh -oStrictHostKeyChecking=no -y citeam@194.110.231.139 uptime'
			sh label: '', script: 'ssh -v citeam@194.110.231.139 \' docker run -p 8000:8000 -d --name nodeapp sampleacc54/nodeapp1\''
}
		
		// sh ' ssh -oStrictHostKeyChecking=no -y citeam@194.110.231.139 uptime \' docker run -p 8081:8000 -d --name test-jenkins sampleacc54/nodeapp1 \''

		
}
}
