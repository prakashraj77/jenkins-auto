pipeline {
    agent any

    environment {
        AWS_ACCOUNT_ID = "799997637318"
        REGION = "ap-south-1"
        REPO_NAME = "myapp-repo"
        IMAGE_TAG = "latest"
    }

    stage('Clone Code') {
    steps {
        git branch: 'main',
            credentialsId: 'github-credentials',  // ← the ID you set in Jenkins credentials
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
                sh '''
                docker tag $REPO_NAME:$IMAGE_TAG \
                $AWS_ACCOUNT_ID.dkr.ecr.$REGION.amazonaws.com/$REPO_NAME:$IMAGE_TAG
                '''
            }
        }

        stage('Push Docker Image to ECR') {
            steps {
                sh '''
                docker push \
                $AWS_ACCOUNT_ID.dkr.ecr.$REGION.amazonaws.com/$REPO_NAME:$IMAGE_TAG
                '''
            }
        }
    }

