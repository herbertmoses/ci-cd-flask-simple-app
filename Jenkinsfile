pipeline {
    agent any

    environment {
        IMAGE_NAME = 'flask-app'
        DOCKER_HUB_REPO = 'mosy778/flask-app'
    }

    stages {
        stage('Clean Workspace') {
            steps {
                echo 'Cleaning workspace...'
                deleteDir()
            }
        }

        stage('Clone Repo') {
            steps {
                sh 'git clone -b herbertmoses-patch-1 https://github.com/herbertmoses/ci-cd-flask-simple-app.git'
                dir('ci-cd-flask-simple-app') {
                    sh 'ls -al'
                }
            }
        }

        stage('Docker Compose Build') {
            steps {
                dir('ci-cd-flask-simple-app') {
                    sh 'docker compose build'
                }
            }
        }

        stage('Run Flask App with Compose') {
            steps {
                dir('ci-cd-flask-simple-app') {
                    sh 'docker compose up -d'
                }
            }
        }

        stage('Tag and Push to Docker Hub') {
            steps {
                script {
                    sh "docker tag ${IMAGE_NAME} ${DOCKER_HUB_REPO}:latest"
                    sh "docker push ${DOCKER_HUB_REPO}:latest"
                }
            }
        }

        stage('Complete') {
            steps {
                echo 'App built, deployed, and pushed to Docker Hub!'
            }
        }
    }
}

