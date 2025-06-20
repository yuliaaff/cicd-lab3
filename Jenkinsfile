pipeline {
    agent any
    environment { 
        PORT = "${env.BRANCH_NAME == 'main' ? '3000' : '3001'}"
        IMAGE_NAME = "${env.BRANCH_NAME == 'main' ? 'nodemain:v1.0' : 'nodedev:v1.0'}"
    }
    tools {
        nodejs 'Node 7.8.0'  
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
                sh 'docker stop $(docker ps -q --filter "expose=${PORT}") && docker rm $(docker ps -aq --filter "expose=${PORT}")'
                sh 'docker run -d --expose ${PORT} -p ${PORT}:${PORT} ${IMAGE_NAME}'
            }
        }
    }
}