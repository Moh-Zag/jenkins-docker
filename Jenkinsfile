pipeline {
    agent any

    environment {
        IMAGE_NAME = 'mohzag/jenkins-flask-app'
        IMAGE_TAG = "${IMAGE_NAME}:${env.BUILD_NUMBER}"
        
    }
    
    stages {

        stage('Setup') {
                agent { docker { image 'python:3.12.0b3-alpine3.18'
    
                label 'docker-on-vas'
            }
            }
            steps {
                sh 'python --version'
                sh 'pwd'
            //    sh "pip install --no-cache-dir -r requirements.txt"
            }
        }

        // stage('Test') {
        //     steps {
        //         sh "pytest"
        //     }
        // }

        stage('Login to docker hub') {

            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-creds', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                sh 'echo ${PASSWORD} | docker login -u ${USERNAME} --password-stdin'}
                echo 'Login successfully'
            }
        }

        stage('Build Docker Image')
        {
            steps
            {
                sh 'docker build -t ${IMAGE_TAG} .'
                echo "Docker image build successfully"
                sh 'docker image ls'
                
            }
        }

        stage('Push Docker Image')
        {
            steps
            {
                sh 'docker push ${IMAGE_TAG}'
                echo "Docker image push successfully"
            }
        }      
    }
    post {
        always{
            cleanWs()
        }
    }
}
