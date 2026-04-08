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
        stage('Dokcer Image Push'){
            steps {
                script {
                    docker.withRegistry('','docker-auth'){
                        oDockImage.push()
                    }
                }
            }
        }
        stage('Deploy Server') {
            steps {
                sshagent(credentials:['Deploy-Privatekey']){
                    sh "ssh -o StrictHostKeyChecking=no ubuntu@43.203.250.150 docker container rm -f sampleweb "
                    sh "ssh -o StrictHostKeyChecking=no ubuntu@43.203.250.150 docker run -d -p 80:80 --name sampleweb ${strDockerImage}"
                }
            }    
        }   
    }
}
