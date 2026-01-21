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



## Deployment

Application deployed on AWS EC2.

Docker container runs as a non-root user.

### Access the application

- Health check: `http://<EC2_PUBLIC_IP>:8000/health`
- Hello endpoint: `http://<EC2_PUBLIC_IP>:8000/hello`

---

## Notes / Assumptions

- Docker and Jenkins are installed on the EC2 instance
- Jenkins user has permission to run Docker commands
- Docker Hub credentials are stored in Jenkins (`docker-hub-creds`)
- EC2 security group allows inbound traffic on port **8000**

---

## CI/CD Workflow

GitHub → Jenkins → Docker Build → Docker Hub Push → EC2 Deployment
