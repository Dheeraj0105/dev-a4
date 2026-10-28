
pipeline {
    agent any

    stages {
        stage('Check Docker') {
            steps {
                bat 'docker --version'
            }
        }

        stage('Build Docker Image') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKERHUB_USER',
                    passwordVariable: 'DOCKERHUB_TOKEN'
                )]) {
                    bat 'docker build -t %DOCKERHUB_USER%/dev-lab-app:latest .'
                }
            }
        }

        stage('Login and Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKERHUB_USER',
                    passwordVariable: 'DOCKERHUB_TOKEN'
                )]) {
                    bat '''
                    @echo off
                    echo %DOCKERHUB_TOKEN% | docker login --username %DOCKERHUB_USER% --password-stdin
                    if errorlevel 1 exit /b 1
                    docker push %DOCKERHUB_USER%/dev-lab-app:latest
                    if errorlevel 1 exit /b 1
                    docker logout
                    '''
                }
            }
        }
    }
}
