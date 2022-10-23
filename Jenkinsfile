pipeline {
    agent any
    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    dokcerapp = docker.build("dav0x64/kube-news:latest", "-f ./src/Dockerfile ./src")
                }
            }
        }
    }
}