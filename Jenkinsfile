pipeline {
    agent any

    environment {
        IMAGE_NAME = 'flask-app'
        DOCKER_HUB_REPO = 'mosy778/flask-app'
        DOCKER_HUB_USERNAME = 'mosy778'
        DOCKER_HUB_PASSWORD = 'Monday!123' // Consider using Jenkins credentials
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

        stage('Install Dependencies') {
            steps {
                dir('ci-cd-flask-simple-app') {
                    sh 'pip install -r requirements.txt'
                }
            }
        }

        stage('Run Tests') {
            steps {
                dir('ci-cd-flask-simple-app') {
                    sh 'pytest tests/'
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
                    sh "echo ${DOCKER_HUB_PASSWORD} | docker login -u ${DOCKER_HUB_USERNAME} --password-stdin"
                    sh "docker tag ${IMAGE_NAME} ${DOCKER_HUB_REPO}:latest"
                    sh "docker push ${DOCKER_HUB_REPO}:latest"
                }
            }
        }

        stage('Complete') {
            steps {
                echo 'App tested, built, deployed, and pushed to Docker Hub!'
            }
        }
    }
}
