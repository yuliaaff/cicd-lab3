pipeline {
    agent any
    environment { 
        PORT = "${env.BRANCH_NAME == 'main' ? '3000' : '3001'}"
        IMAGE_NAME = "${env.BRANCH_NAME == 'main' ? 'nodemain:v1.0' : 'nodedev:v1.0'}"
        PATH = "/usr/local/bin:${env.PATH}"
    }
    tools {
        nodejs 'node'
    }
    stages {
        stage('Build'){
            steps{
                sh 'npm install'
            }
        }
        stage('Test'){
            steps{
                sh 'npm test'
            }
        }
        stage('Docker Build'){
            steps{
                sh "docker build -t ${IMAGE_NAME} ."
            }
        }
        stage('Deploy'){
            steps{
                sh '''
                    containers_to_stop=$(docker ps -q --filter "ancestor=${IMAGE_NAME}")
                    if [ ! -z "$containers_to_stop" ]; then
                        docker stop $containers_to_stop
                        docker rm $containers_to_stop
                    else
                        echo "No running containers found for image: ${IMAGE_NAME}"
                    fi
                    '''
                sh "docker run -d --expose ${PORT} -p ${PORT}:${PORT} ${IMAGE_NAME}"
            }
        }
    }
}