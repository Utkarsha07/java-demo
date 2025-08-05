pipeline {
    agent any

    environment {
        AWS_REGION = 'ap-east-1'
        ECR_REPO = '812982729567.dkr.ecr.us-east-1.amazonaws.com/java-ecr-repo'
        IMAGE_TAG = "latest"
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/yourname/java-demo.git'
            }
        }

        stage('Build Java App') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t $ECR_REPO:$IMAGE_TAG ."
            }
        }

        stage('Login to ECR') {
            steps {
                sh "aws ecr get-login-password --region $AWS_REGION | docker login --username AWS --password-stdin $ECR_REPO"
            }
        }

        stage('Push to ECR') {
            steps {
                sh "docker push $ECR_REPO:$IMAGE_TAG"
            }
        }
    }
}
