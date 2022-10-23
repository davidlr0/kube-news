pipeline {
    agent any
    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    dockerapp = docker.build("dav0x64/kube-news:${env.BUILD_ID}", "-f ./src/Dockerfile ./src")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com/', 'dockerhub'){
                        dockerapp.push('latest')
                        dockerapp.push("${env.BUILD_ID}")
                    }
                    
                }
            }
        }

        stage('Deploy Kubernetes') {
            steps {
                environment {
                    tag_version = "${env.BUILD_ID}"
                }
                script {
                    withKubeConfig([credentialsId: 'kubeconfig']) {
                        sh 'sed -i "s/{{TAG}}/$tag_version/g" ./k8s/manifest.yaml' 
                        sh 'kubectl apply -f ./k8s/manifest.yaml'
                    }
                }
            }
        }
    }
}
