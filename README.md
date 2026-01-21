# FastAPI Application Deployment with Docker and CI/CD

## Project Overview
This project demonstrates containerization and deployment of a Python FastAPI application using Docker. A CI/CD pipeline is set up using Jenkins to automate code checkout, Docker image build, push to Docker Hub, and deployment on an AWS EC2 instance.

The application exposes two endpoints:
- `GET /health` → returns application health status
- `GET /hello` → returns a JSON message
- Application runs on port 8000.

## Technology Stack
- **Backend:** FastAPI (Python 3.11)
- **Containerization:** Docker
- **CI/CD:** Jenkins
- **Cloud Deployment:** AWS EC2
- **Version Control:** GitHub
- **Docker Registry:** Docker Hub

---

## Project Structure
├── Dockerfile
├── Jenkinsfile
├── main.py
├── requirements.txt


---

## Dockerization

### Dockerfile
FROM python:3.11-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY main.py .
RUN useradd -m appuser
USER appuser
EXPOSE 8000
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]

Explanation:
FROM python:3.11-slim → minimal Python image.
WORKDIR /app → sets working directory.
COPY requirements.txt . → copy dependencies.
RUN pip install -r requirements.txt → install dependencies.
COPY main.py . → copy app code.
RUN useradd -m appuser → create non-root user.
USER appuser → run as non-root user.
EXPOSE 8000 → expose app port.
CMD [...] → start FastAPI server.


Jenkinsfile:
pipeline {
    agent any
    environment {
        IMAGE_NAME = "mokshitgupta/fastapi-app"
        IMAGE_TAG = "latest"
    }
    stages {
        stage('Checkout Code') {
            steps { git branch: 'main', url: 'https://github.com/mokshitgupta/devops-intern-task.git' }
        }
        stage('Build Docker Image') {
            steps { sh 'docker build -t $IMAGE_NAME:$IMAGE_TAG .' }
        }
        stage('Docker Hub Login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                }
            }
        }
        stage('Push Image to Docker Hub') { steps { sh 'docker push $IMAGE_NAME:$IMAGE_TAG' } }
        stage('Deploy Container') {
            steps {
                sh '''
                docker stop fastapi_container || true
                docker rm fastapi_container || true
                docker run -d -p 8000:8000 --name fastapi_container --restart unless-stopped $IMAGE_NAME:$IMAGE_TAG
                '''
            }
        }
    }
    post {
        success { echo "Pipeline completed successfully!" }
        failure { echo "Pipeline failed. Check logs." }
    }
}

##Deployment
Application deployed on AWS EC2.
Docker container runs as non-root user.
Access the app via:
Health check: http://<EC2_PUBLIC_IP>:8000/health
Hello endpoint: http://<EC2_PUBLIC_IP>:8000/hello

##Notes / Assumptions
Docker and Jenkins installed on EC2.
Jenkins user has permission to run Docker commands.
Docker Hub credentials stored in Jenkins (docker-hub-creds).
EC2 security group allows traffic on port 8000.

##CI/CD Workflow
GitHub → Jenkins → Docker Build → Docker Hub Push → EC2 Deployment

##Conclusion
FastAPI application containerized with Docker.
CI/CD pipeline automates build, push, deployment.
Application accessible publicly via EC2 IP on port 8000.
