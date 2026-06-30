def branch = "main"
def remote = "origin"
def directory = "~/app/wayshub-backend"
def server = "ademulyana@103.31.38.220"
def cred = "ssh-key-ademulyana"

def imageName = "oneslicedbread/wayshub-backend:staging"

pipeline {
    agent any

    stages {
        stage('Repository Pull') {
            steps {
                sshagent([cred]) {
                    sh """
                    ssh -o StrictHostKeyChecking=no ${server} '
                    cd ${directory}
                    git pull ${remote} ${branch}
                    '
                    """
                }
            }
        }

        stage('Docker Build') {
            steps {
                sshagent([cred]) {
                    sh """
                    ssh -o StrictHostKeyChecking=no ${server} '
                    cd ${directory}
                    docker build -t ${imageName} .
                    '
                    """
                }
            }
        }

        stage('Docker Deploy') {
            steps {
                sshagent([cred]) {
                    sh """
                    ssh -o StrictHostKeyChecking=no ${server} '
                    docker stop wayshub-backend-staging || true
                    docker rm wayshub-backend-staging || true
                    docker run -d \
                        --name wayshub-backend-staging \
                        -p 3101:5000 \
                        ${imageName}
                    '
                    """
                }
            }
        }
    }
}
