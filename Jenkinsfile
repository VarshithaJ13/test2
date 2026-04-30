pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "varshithaj/imagev"
        TAG = "v1"
    }

    stages {
        stage('Clone Code') {
            steps {
                
                git branch: 'main', url: 'https://github.com/VarshithaJ13/test2.git'
            }
        }

        stage('Build Docker Image') {
    steps {
        
        bat '"C:\\Program Files\\Docker\\Docker\\resources\\bin\\docker.exe" build -t %DOCKER_IMAGE%:%TAG% .'
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
                sh "docker run -d -p 9090:80 $DOCKER_IMAGE:$TAG"
            }
        }
    }
}
