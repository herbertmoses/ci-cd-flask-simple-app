
pipeline {
    agent any
    stages {
        stage('Clone Repo') {
            steps {
                git branch: 'main', url: 'https://github.com/herbertmoses/ci-cd-flask-simple-app.git'
            }
        }
        stage('Install Dependencies') {
            steps {
                sh '''
                echo "Setting up Python virtual environment..."
                python3 -m venv venv
                echo "Activating virtual environment..."
                bash -c "source venv/bin/activate && pip install -r requirements.txt"
                '''
            }
        }
        stage('Run Flask App') {
            steps {
                sh '''
                echo "Activating virtual environment..."
                bash -c "source venv/bin/activate && echo Running flask application && nohup python app.py > flask.log 2>&1 &"
                '''
            }
        }
        stage('complete') {
            steps {
                echo 'Job complete!'
            }
        }
    }
}
