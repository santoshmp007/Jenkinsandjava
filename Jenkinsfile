pipeline {
    agent any
    environment {
        GIT_REPO = 'https://github.com/Ajayrichard/Jenkinsandjava.git'https://github.com/santoshmp007/Jenkinsandjava/tree/main
        AWS_REGION = 'ap-south-1'
        ECR_REPO_NAME = 'san'
        ECR_REPO_URI = '345594572626.dkr.ecr.ap-south-1.amazonaws.com/san'
        IMAGE_TAG = 'latest'
        AWS_ACCOUNT_ID = '345594572626'
        IMAGE_URI = "${ECR_REPO_URI}:${IMAGE_TAG}"
        EKS_CLUSTER = 'my-eks-cluster'
    }

    stages {
        stage('Configure AWS Credentials') {
            steps {
                script {
                    sh '''
                        echo "Setting up AWS credentials for Jenkins..."
                        mkdir -p /var/lib/jenkins/.aws
                        echo "[default]" > /var/lib/jenkins/.aws/credentials
                        echo "aws_access_key_id=AKIAVA5YKV5JCXGWOMNE" >> /var/lib/jenkins/.aws/credentials
                        echo "aws_secret_access_key=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" >> /var/lib/jenkins/.aws/credentials
                        chown -R jenkins:jenkins /var/lib/jenkins/.aws
                    '''
                }
            }
        }

        stage('Clone Repository') {
            steps {
                git url: "${GIT_REPO}", branch: 'main'
            }
        }

        stage('Build') {
            steps {
                script {
                    sh '''
                        echo "Building Java application..."
                        mvn clean -B -Denforcer.skip=true package
                    '''
                }
            }
        }

        stage('Login to AWS ECR') {
            steps {
                script {
                    sh '''
                        echo "Logging into AWS ECR..."
                        aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 345594572626.dkr.ecr.ap-south-1.amazonaws.com
                    '''
                }
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    sh '''
                        echo "Building Docker image..."
                        docker build -t ${IMAGE_URI} .
                    '''
                }
            }
        }
        stage('Push Docker Image to ECR') {
            steps {
                script {
                    sh '''
                        echo "Pushing Docker image to ECR..."
                        docker push ${IMAGE_URI}
                    '''
                }
            }
        }
    }
}
