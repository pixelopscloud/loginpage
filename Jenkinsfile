pipeline {
    agent any
    
    environment {
        DOCKER_HUB_REPO = 'pixelopscloud'
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
                    sh "docker build -t ${DOCKER_HUB_REPO}/${BACKEND_IMAGE}:${IMAGE_TAG} ."
                    sh "docker tag ${DOCKER_HUB_REPO}/${BACKEND_IMAGE}:${IMAGE_TAG} ${DOCKER_HUB_REPO}/${BACKEND_IMAGE}:latest"
                }
            }
        }
        
        stage('Build Frontend') {
            steps {
                echo 'Building Frontend Docker Image...'
                dir('frontend') {
                    sh "docker build -t ${DOCKER_HUB_REPO}/${FRONTEND_IMAGE}:${IMAGE_TAG} ."
                    sh "docker tag ${DOCKER_HUB_REPO}/${FRONTEND_IMAGE}:${IMAGE_TAG} ${DOCKER_HUB_REPO}/${FRONTEND_IMAGE}:latest"
                }
            }
        }
        
        stage('Push to Docker Hub') {
            steps {
                echo 'Pushing images to Docker Hub...'
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                    sh "docker push ${DOCKER_HUB_REPO}/${BACKEND_IMAGE}:${IMAGE_TAG}"
                    sh "docker push ${DOCKER_HUB_REPO}/${BACKEND_IMAGE}:latest"
                    sh "docker push ${DOCKER_HUB_REPO}/${FRONTEND_IMAGE}:${IMAGE_TAG}"
                    sh "docker push ${DOCKER_HUB_REPO}/${FRONTEND_IMAGE}:latest"
                }
            }
        }
    }
    
    post {
        success {
            echo '✅ Pipeline completed! Images pushed to Docker Hub!'
        }
        failure {
            echo '❌ Pipeline failed!'
        }
    }
}
