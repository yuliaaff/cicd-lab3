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
                sh 'docker stop $(docker ps -q --filter "ancestor=${IMAGE_NAME}") && docker rm $(docker ps -aq --filter "ancestor=${IMAGE_NAME}")'
                sh 'docker run -d --expose ${PORT} -p ${PORT}:${PORT} ${IMAGE_NAME}'
            }
        }
    }
}