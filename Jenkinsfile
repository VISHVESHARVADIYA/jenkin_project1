pipeline {
    agent any

    environment {
        // Get AWS credentials from Jenkins Credentials Manager
        AWS_ACCOUNT_ID = '036919103563'
        AWS_DEFAULT_REGION = 'us-east-1'
        ECR_REPO_NAME = 'flask-ecs-app'
        ECR_REGISTRY = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com"
        IMAGE_TAG = "latest"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'YOUR_GITHUB_REPO_URL'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${ECR_REGISTRY}/${ECR_REPO_NAME}:${IMAGE_TAG}", ".")
                }
            }
        }

        stage('Push to ECR') {
            steps {
                script {
                    // Login to ECR and push the image
                    docker.withRegistry("https://_,"aws:ecr:${AWS_DEFAULT_REGION}:flask-ecs-app") {
                        docker.image("${ECR_REGISTRY}/${ECR_REPO_NAME}:${IMAGE_TAG}").push()
                    }
                }
            }
        }

        stage('Deploy to ECS') {
            steps {
                script {
                    // Use AWS CLI to update the service and force a new deployment
                    sh "aws ecs update-service --cluster flask-cluster --service flask-service --force-new-deployment"
                }
            }
        }
    }
}
