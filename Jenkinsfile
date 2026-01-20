pipeline {
    agent any

    environment {
        IMAGE_NAME = "fastapi-app"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/mokshitgupta/devops-intern-task.git'
            }
        }

        stage('Lint') {
            steps {
                sh 'pip3 install fastapi uvicorn flake8'
                sh 'flake8 app || true'
            }
        }

        stage('Build') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                docker stop fastapi_container || true
                docker rm fastapi_container || true
                docker run -d -p 8000:8000 --name fastapi_container $IMAGE_NAME
                '''
            }
        }
    }
}
