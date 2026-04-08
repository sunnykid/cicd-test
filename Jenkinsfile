pipeline {
	agent any

    environment {
        strDockerImage="sunnykid7/cicd-test:0.1"
    }
	stages {
	    stage('Github Pull') {
 	       steps {
	           git branch: 'main',url:'https://github.com/sunnykid/cicd-test.git'
	       } 
        }
        stage('Docker Image Build') {
            steps {
                script {
                    oDockImage = docker.build(strDockerImage,"-f Dockerfile .")
                }
            }
	    }
        stage('Deploy Server') {
            steps {
                sshagent(credentials:['Deploy-Privatekey']){
                    sh "scp -o StrictHostKeyChecking=no index.html ubuntu@43.203.250.150:/home/ubuntu/"
                    sh "ssh -o StrictHostKeyChecking=no ubuntu@43.203.250.150 sudo cp /home/ubuntu/index.html /var/www/html"
                }
            }    
        }   
    }
}
