pipeline {
    agent any
    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/YOUR_GITHUB_USERNAME/laravel-three-tier.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    // Build image using local docker daemon
                    sh 'docker build -t your-dockerhub-username/laravel-app:latest .'
                }
            }
        }
        stage('Push to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USER')]) {
                    sh "echo $DOCKER_PASSWORD | docker login -u $DOCKER_USER --password-stdin"
                    sh 'docker push your-dockerhub-username/laravel-app:latest'
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f k8s/mysql-deployment.yaml'
                sh 'kubectl apply -f k8s/laravel-deployment.yaml'
            }
        }
    }
}
