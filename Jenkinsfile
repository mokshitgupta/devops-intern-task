pipeline {
    agent any

    environment {
        IMAGE_NAME = "fastapi-app"
        CONTAINER_NAME = "fastapi_container"
        PORT = "8000"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/mokshitgupta/devops-intern-task.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t $IMAGE_NAME ."
            }
        }

        stage('Deploy Container') {
            steps {
                sh """
                docker stop $CONTAINER_NAME || true
                docker rm $CONTAINER_NAME || true
                docker run -d -p $PORT:8000 --name $CONTAINER_NAME --restart unless-stopped $IMAGE_NAME
                """
            }
        }
    }

    post {
        success {
            echo "FastAPI app deployed successfully at port $PORT"
        }
        failure {
            echo "Pipeline failed. Check logs."
        }
    }
}

