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
