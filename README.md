CI/CD Flask Simple App

Project Overview

This project demonstrates a complete CI/CD pipeline using:
Flask
Docker
Jenkins
Kubernetes
DockerHub
GitHub

Architecture
Developer → GitHub → Jenkins → Docker → DockerHub → Kubernetes

Local Run (Without Docker)
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
python app.py
Access:
http://localhost:5000

Run With Docker
Build image:
docker build -t flask-app .
Run:
docker run -p 5000:5000 flask-app

Run With Docker Compose
docker-compose up --build

Jenkins Setup
Install Jenkins
Install Plugins:
Docker Pipeline
Kubernetes CLI
Git
Create Pipeline Job
Configure SCM (GitHub repo)
Run pipeline

Kubernetes Deployment
Start Minikube:
minikube start
Deploy:
kubectl apply -f K8s/
Verify:
kubectl get all -n flask-app-namespace
Access Service:
minikube service flask-app-service -n flask-app-namespace

CI/CD Flow
Code push to GitHub
Jenkins triggers pipeline
Runs tests
Builds Docker image
Pushes image to DockerHub
Deploys to Kubernetes
App becomes accessible via NodePort
