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
                    docker build -t wayshub-be .
                    exit
                    EOF"""
                }
            }
        }
        stage('docker run'){
            steps{
                sshagent([cred]){
                    sh """ssh -o StrictHostKeyChecking=no ${server} << EOF
                    docker stop dennisjenkins-be || true
                    docker rm dennisjenkins-be || true
                    docker run -d -p 5000:5000 --name dennisjenkins-be wayshub-be
                    exit
                    EOF"""
                }
            }
        }
    }
}
