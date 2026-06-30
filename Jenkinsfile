def branch = "main"
def remote = "origin"
def directory = "~/jenkins/wayshub-backend"
def server = "devops27@103.193.178.218"
def cred = "ssh-key-devops27"

pipeline{
    agent any
    stages{
        stage('repo pull'){
            steps{
                sshagent([cred]){
                    sh """ssh -o StrictHostKeyChecking=no ${server} << EOF
                    cd ${directory}
                    git pull ${remote} ${branch}
                    exit
                    EOF"""
                }
            }
        }
        stage('docker build'){
            steps{
                sshagent([cred]){
                    sh """ssh -o StrictHostKeyChecking=no ${server} << EOF
                    cd ${directory}
                    docker build -t djsn98/devops27-wayshub-backend-2:v1.0 .
                    exit
                    EOF"""
                }
            }
        }
	stage('docker push'){
            steps{
                sshagent([cred]){
                    sh """ssh -o StrictHostKeyChecking=no ${server} << EOF
                    echo "susah13234" | docker login -u "djsn98" --password-stdin
		    docker push djsn98/devops27-wayshub-backend-2:v1.0 
                    exit
                    EOF"""
                }
            }
        }
        stage('docker run'){
            steps{
                sshagent([cred]){
                    sh """ssh -o StrictHostKeyChecking=no ${server} << EOF
                    docker stop backend-1 || true
                    docker rm backend-1 || true
                    docker run -d -p 5000:5000 --network app_ade-dennis-prod --name backend-1 djsn98/devops27-wayshub-backend-2:v1.0
                    exit
                    EOF"""
                }
            }
        }
    }
}
