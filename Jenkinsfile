pipeline {
    agent any
    
    environment {
        BACKEND_IMAGE = 'login-backend'
        FRONTEND_IMAGE = 'login-frontend'
        IMAGE_TAG = "${BUILD_NUMBER}"
    }
    
    stages {
        stage('Checkout') {
            steps {
                echo 'Pulling code from GitHub...'
                git branch: 'main', 
                    url: 'https://github.com/pixelopscloud/loginpage.git',
                    credentialsId: 'github-credentials'
            }
        }
        
        stage('Build Backend') {
            steps {
                echo 'Building Backend Docker Image...'
                dir('backend') {
                    sh "docker build -t ${BACKEND_IMAGE}:${IMAGE_TAG} ."
                    sh "docker tag ${BACKEND_IMAGE}:${IMAGE_TAG} ${BACKEND_IMAGE}:latest"
                }
            }
        }
        
        stage('Build Frontend') {
            steps {
                echo 'Building Frontend Docker Image...'
                dir('frontend') {
                    sh "docker build -t ${FRONTEND_IMAGE}:${IMAGE_TAG} ."
                    sh "docker tag ${FRONTEND_IMAGE}:${IMAGE_TAG} ${FRONTEND_IMAGE}:latest"
                }
            }
        }
        
        stage('List Images') {
            steps {
                echo 'Listing Docker Images...'
                sh 'docker images | grep login'
            }
        }
        
        stage('Deploy Info') {
            steps {
                echo "Backend Image: ${BACKEND_IMAGE}:${IMAGE_TAG}"
                echo "Frontend Image: ${FRONTEND_IMAGE}:${IMAGE_TAG}"
                echo 'Ready for Kubernetes deployment!'
            }
        }
    }
    
    post {
        success {
            echo '✅ Pipeline completed successfully!'
        }
        failure {
            echo '❌ Pipeline failed!'
        }
    }
}
