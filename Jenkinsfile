pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-id')
        DOCKER_IMAGE = "vijaya123qw/task6:latest"
        AWS_REGION = "ap-south-1"
        CLUSTER_NAME = "vijayacluster"   // 👈 change this
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Vijayasaka-112/task6.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }
        stage('Push Docker Image') {
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
                sh 'docker push $DOCKER_IMAGE'
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                aws eks update-kubeconfig --region $AWS_REGION --name $CLUSTER_NAME
                kubectl apply -f k8s/deployment.yml
                kubectl apply -f k8s/service.yml
                kubectl apply -f k8s/ingress.yml
                '''
            }
        }
    }
}
