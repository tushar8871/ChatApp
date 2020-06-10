pipeline {
	agent any
	stages {
		stage('Clone Repository') {
			steps {
				sh ''' #! /bin/bash
				ssh -i /var/lib/jenkins/.ssh/id_rsa root@3.7.68.211 '
				sudo rm -rf chatApp13/
				'
				scp -r /var/lib/jenkins/workspace/chatApp13 root@3.7.68.211:
				'''
			}
		}
		stage('Build Image') {
			steps {
				sh ''' #! /bin/bash
				ssh -i /var/lib/jenkins/.ssh/id_rsa root@3.7.68.211 '
				cd chatApp13/
				$(aws ecr get-login --registry-ids 760496128264 --no-include-email --region ap-south-1)
				#docker-compose down 
				docker stop $(docker ps -a -q)
				docker rm $(docker ps -a -q)
				docker rmi -f $(docker images -a -q dangling=true)
				docker-compose up -d
				'
				'''
			}
		}
		stage('Deploy') {
			steps {
				sh ''' #! /bin/bash
				echo Deploy Successfull
				'''
			}
		}
		stage('Push Image') {
			steps { 
				sh ''' #! /bin/bash
				ssh -i /var/lib/jenkins/.ssh/id_rsa root@3.7.68.211 '
				docker tag chatapp13_chat 760496128264.dkr.ecr.ap-south-1.amazonaws.com/chatapp:chatapp13_chat
				docker push 760496128264.dkr.ecr.ap-south-1.amazonaws.com/chatapp:chatapp13_chat
				'
				'''
			}
		}

	}
	post {
		always {
			echo 'Stage is success'
		}
	}
}



