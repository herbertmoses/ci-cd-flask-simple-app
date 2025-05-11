pipeline {
    agent any

    environment {
        IMAGE_NAME = 'flask-app'
        DOCKER_HUB_REPO = 'mosy778/flask-app'
        DOCKER_HUB_USERNAME = 'mosy778'
        DOCKER_HUB_PASSWORD = 'Monday!123' // Use Jenkins credentials ideally
        KUBERNETES_CLUSTER_NAME = 'minikube'  // Set the name of your Kubernetes cluster
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
                    sh "echo ${DOCKER_HUB_PASSWORD} | docker login -u ${DOCKER_HUB_USERNAME} --password-stdin"
                    sh "docker tag ${IMAGE_NAME} ${DOCKER_HUB_REPO}:latest"
                    sh "docker push ${DOCKER_HUB_REPO}:latest"
                }
            }
        }

        stage('Deploy with Docker Swarm') {
            steps {
                script {
                    sh 'docker swarm init || true'  // initialize swarm if not already
                    sh '''
                    docker service rm flask_service || true

                    docker service create \
                        --name flask_service \
                        --publish 5000:5000 \
                        --replicas 2 \
                        ${DOCKER_HUB_REPO}:latest
                    '''
                }
            }
        }

        stage('Deploy with Kubernetes') {
            steps {
                script {
                // Run kubectl commands on the host machine
                sh '''
                kubectl config use-context minikube
                kubectl apply -f ci-cd-flask-simple-app/k8s/namespace.yaml || true
                kubectl apply -f ci-cd-flask-simple-app/k8s/deployment.yaml
                kubectl apply -f ci-cd-flask-simple-app/k8s/service.yaml
                kubectl rollout status deployment flask-app-deployment
                kubectl get svc flask-app-service
                '''
                }
            }
        }

        stage('Complete') {
            steps {
                echo 'App built, pushed to Docker Hub, deployed with Docker Swarm, and deployed with Kubernetes!'
            }
        }
    }
}
