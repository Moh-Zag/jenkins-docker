pipeline {
    agent { docker { image 'python:3.12.0b3-alpine3.18'
    
           label 'docker-on-vas'
    }
    }
    environment {
        IMAGE_NAME = 'mohzag/jenkins-flask-app'
        IMAGE_TAG = "${IMAGE_NAME}:${env.BUILD_NUMBER}"
        
    }

    
    stages {

        stage('Setup') {
            steps {
                sh 'python --version'
                sh 'pwd'
                // sh "pip install -r requirements.txt"
            }
        }
        // stage('Test') {
        //     steps {
        //         sh "pytest"
        //     }
        // }

        stage('Login to docker hub') {

            agent { docker { image 'docker:latest'
    
           label 'docker-on-vas'
    }
    }
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-creds', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                sh 'echo ${PASSWORD} | docker login -u ${USERNAME} --password-stdin'}
                echo 'Login successfully'
            }
        }

        stage('Build Docker Image')
        {
            agent { docker { image 'docker:latest'
    
           label 'docker-on-vas'
    }
    }
            steps
            {
                sh 'docker build -t ${IMAGE_TAG} .'
                echo "Docker image build successfully"
                sh 'docker image ls'
                
            }
        }

        stage('Push Docker Image')
        {
            agent { docker { image 'docker:latest'
    
           label 'docker-on-vas'
    }
    }
            steps
            {
                sh 'docker push ${IMAGE_TAG}'
                echo "Docker image push successfully"
            }
        }      
    }
}