pipeline {
    agent any
    stages {
        stage('Build Docker Image') {
            steps {
                sh '''
                docker build -t flask-app .
                '''
            }
        }
        stage('Run Flask Container') {
            steps {
                sh '''
                docker run -d -p 5000:5000 flask-app
                '''
            }
        }
        stage('Complete') {
            steps {
                echo 'Deployment successful!'
            }
        }
    }
}
