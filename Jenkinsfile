def branch = "main"
def nama_repository = "origin"
def dir = "~/housy-frontend/"
def credential = 'housy_appserver'
def server = 'housy_appserver@103.134.154.8'
def docker_image = 'reiya24/housy-frontend'
def nama_container = 'frontend'

pipeline {
   agent any

    stages {
        stage('pull repository dari github ') {
            steps {
                sshagent([credential]){
                    sh"""
                    ssh -o StrictHostKeyChecking=no ${server} << EOF
                    echo "Pulling Housy frontend Repository"
                    cd ${dir}
                    docker container stop ${nama_container}
                    docker container rm ${nama_container}
                    git pull ${nama_repository} ${branch}
                    exit
                    EOF"""
                }
            }
        }

        stage('build image frontend') {
            steps {
                sshagent([credential]){
                    sh"""ssh -o StrictHostKeyChecking=no ${server} << EOF
                    echo "Building Image"
                    cd ${dir}
                    docker build -t ${docker_image}:latest .
                    exit
                    EOF"""
                }
            }
        }

        stage('jalankan docker compose') {
            steps {
                sshagent([credential]){
                    sh"""ssh -o StrictHostKeyChecking=no ${server} << EOF
                    cd ${dir}
                    docker compose -f docker-compose.yml up -d
                    exit
                    EOF
                    """
                }
            }
        }

        stage('push image ke dockerhub') {
            steps {
                sshagent([credential]){
                    sh"""
                    ssh -o StrictHostKeyChecking=no ${server} << EOF
                    cd ${dir}
                    docker image push ${docker_image}:latest
                    exit
                    EOF"""
                }
            }
        }
    }
}
