pipeline {
    agent any
    environment {
        DOCKERHUB_USERNAME = 'mosy778'
        IMAGE_NAME = 'flask-app'
    }
    stages {
        stage('Docker Compose Build') {
            steps {
                sh 'docker-compose build'
            }
        }
        stage('Run Flask App with Compose') {
            steps {
                sh 'docker-compose up -d'
            }
        }
        stage('Tag and Push to Docker Hub') {
            steps {
                sh '''
                docker tag ${IMAGE_NAME} ${DOCKERHUB_USERNAME}/${IMAGE_NAME}:latest
                docker push ${DOCKERHUB_USERNAME}/${IMAGE_NAME}:latest
                '''
            }
        }
        stage('Complete') {
            steps {
                echo 'App built, deployed, and pushed to Docker Hub!'
            }
        }
    }
}
