pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "varshithaj/imagev"
        TAG = "v1"
        // Adding the path here fixes the "executable not found" errors once and for all
        PATH = "C:\\Program Files\\Docker\\Docker\\resources\\bin;C:\\Program Files\\Git\\bin;${env.PATH}"
    }

    stages {
        stage('Clone Code') {
            steps {
                git branch: 'main', url: 'https://github.com/VarshithaJ13/test2.git'
            }
        }

        stage('Build Docker Image') {
            steps {
               
                bat "docker build -t %DOCKER_IMAGE%:%TAG% ."
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', 
                usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                  
                    bat "echo %PASSWORD%| docker login -u %USERNAME% --password-stdin"
                }
            }
        }

        stage('Push Image') {
            steps {
                bat "docker push %DOCKER_IMAGE%:%TAG%"
            }
        }

        stage('Run Container') {
            steps {
                
                bat "docker run -d -p 8080:80 --name my_web_app %DOCKER_IMAGE%:%TAG%"
            }
        }
    }
}