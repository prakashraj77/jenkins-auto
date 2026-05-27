pipeline {
    agent any

    environment {
        AWS_ACCOUNT_ID = "799997637318"  // replace with real value
        REGION = "ap-south-1"
        REPO_NAME = "myapp-repo"                 // replace with real value
        IMAGE_TAG = "latest"
    }

    stages {

        stage('Clone Code') {
            steps {
                git branch: 'main',
                    credentialsId: 'github-credentials',
                    url: 'https://github.com/prakashraj77/jenkins-auto.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $REPO_NAME:$IMAGE_TAG .'
            }
        }

        stage('Login to Amazon ECR') {
            steps {
                sh '''
                aws ecr get-login-password --region $REGION | \
                docker login --username AWS --password-stdin \
                $AWS_ACCOUNT_ID.dkr.ecr.$REGION.amazonaws.com
                '''
            }
        }

        stage('Tag Docker Image') {
            steps {
                sh 'docker tag $REPO_NAME:$IMAGE_TAG $AWS_ACCOUNT_ID.dkr.ecr.$REGION.amazonaws.com/$REPO_NAME:$IMAGE_TAG'
            }
        }

        stage('Push Docker Image to ECR') {
            steps {
                sh 'docker push $AWS_ACCOUNT_ID.dkr.ecr.$REGION.amazonaws.com/$REPO_NAME:$IMAGE_TAG'
            }
        }

    }
}
