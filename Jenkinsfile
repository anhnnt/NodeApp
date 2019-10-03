node {
    def app

    stage('Clone repository') {
        /* Cloning the Repository to our Workspace */

        checkout scm
    }

    stage('Build image') {
        /* This builds the actual image */

        app = docker.build("nonah/nodeapp")
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
        docker.withRegistry('https://registry.hub.docker.com', 'dockerHub') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
            } 
                echo "Trying to Push Docker Build to DockerHub"
    }
	stage('Run container on DevServer'){
		// def dockerRun = "docker run -p 8080:8080 -d --name test-jenkins nonah/nodeapp"
		ssh 'docker run -p 8080:8080 -d --name test-jenkins nonah/nodeapp'
}
}
