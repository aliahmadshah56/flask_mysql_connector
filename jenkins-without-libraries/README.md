# Flask MySQL Connector - Dockerized CI/CD Pipeline

A complete DevOps practice project demonstrating how to containerize a Flask application and automate the build and deployment process using **Docker, Docker Compose, Jenkins, GitHub Webhooks, and Docker Hub**.

---

## 🚀 Project Overview

This project demonstrates a complete CI/CD workflow:

1. Developer pushes code to GitHub
2. GitHub Webhook triggers Jenkins automatically
3. Jenkins agent clones the latest source code
4. Jenkins builds a Docker image
5. Jenkins runs container deployment using Docker Compose
6. Docker image is pushed to Docker Hub

---

# 🏗️ Architecture

```
Developer
    |
    | git push
    |
    v
GitHub Repository
    |
    | Webhook Trigger
    |
    v
Jenkins Server
    |
    | Pipeline Execution
    |
    v
Jenkins Agent (Ubuntu EC2)
    |
    | Docker Build
    |
    v
Docker Image
    |
    | Push
    |
    v
Docker Hub
    |
    | Deploy
    |
    v
Docker Container
```

---

# 🛠️ Technologies Used

| Technology      | Purpose                |
| --------------- | ---------------------- |
| Python          | Application language   |
| Flask           | Web framework          |
| Docker          | Containerization       |
| Docker Compose  | Container management   |
| Jenkins         | CI/CD automation       |
| GitHub          | Source code management |
| GitHub Webhooks | Automation trigger     |
| Docker Hub      | Image registry         |
| AWS EC2         | Cloud infrastructure   |

---

# 📂 Project Structure

```
flask_mysql_connector/
│
├── Dockerfile
├── docker-compose.yml
├── Jenkinsfile
├── requirements.txt
├── setup.py
├── README.md
│
├── examples/
│   └── example_app.py
│
└── flask_mysql_connector/
    ├── __init__.py
    ├── flask_mysql_connector.py
    └── params.py
```

---

# 🐳 Docker Configuration

## Dockerfile

The application image is created using Python 3.9 slim image.

Workflow:

```
Python Base Image
        |
Install Dependencies
        |
Copy Application Code
        |
Run Flask Application
```

Build Docker image:

```bash
docker build -t flask-app:latest .
```

Run container:

```bash
docker run -d -p 5000:5000 flask-app:latest
```

---

# 🐋 Docker Compose

Docker Compose manages application deployment.

Example:

```yaml
services:
  flask:
    container_name: flask
    build:
      context: .
    ports:
      - "5000:5000"
```

Start application:

```bash
docker compose up -d --build
```

Stop application:

```bash
docker compose down
```

---

# ⚙️ Jenkins CI/CD Pipeline

The Jenkins pipeline performs:

## Stage 1 - Code Checkout

Jenkins pulls the latest code from GitHub.

```groovy
git url: "repository-url",
branch: "master"
```

---

## Stage 2 - Docker Permission Check

The Jenkins agent verifies Docker access:

```bash
whoami
groups
docker ps
```

The Jenkins user must belong to the Docker group.

Example:

```bash
sudo usermod -aG docker ubuntu
```

---

## Stage 3 - Build Docker Image

Pipeline creates Docker image:

```bash
docker build -t flask-app:latest .
```

---

## Stage 4 - Deployment

Application deployment using Docker Compose:

```bash
docker compose down

docker compose up --build -d
```

---

## Stage 5 - Push Image

Jenkins authenticates with Docker Hub and pushes image:

```bash
docker login

docker tag flask-app:latest username/flask-app:latest

docker push username/flask-app:latest
```

---

# 🔑 Jenkins Agent Configuration

Jenkins uses a separate Ubuntu EC2 instance as an agent.

Configuration:

```
Node Name:
flask-agent

Remote Directory:
/home/ubuntu/jenkins

Label:
flask

Launch Method:
SSH
```

The agent provides:

* Docker build environment
* Application deployment environment
* Independent execution machine

---

# 🔄 GitHub Webhook Integration

GitHub webhook automatically triggers Jenkins after every push.

Webhook URL:

```
http://<JENKINS_PUBLIC_IP>:8080/github-webhook/
```

Flow:

```
Git Push
   |
   v
GitHub Webhook
   |
   v
Jenkins Pipeline Trigger
```

---

# ☁️ AWS Infrastructure

The project uses AWS EC2 instances:

## Jenkins Server

Responsibilities:

* Manage pipelines
* Receive GitHub webhook
* Control CI/CD workflow

## Jenkins Agent

Responsibilities:

* Execute pipeline jobs
* Build Docker images
* Run containers

---

# 🚦 Running Locally

Clone repository:

```bash
git clone https://github.com/aliahmadshah56/flask_mysql_connector.git
```

Move into project:

```bash
cd flask_mysql_connector
```

Build:

```bash
docker build -t flask-app .
```

Run:

```bash
docker run -p 5000:5000 flask-app
```

Open:

```
http://localhost:5000
```

---

# 🔐 Important Notes

* Never store Docker Hub passwords inside Jenkinsfile.
* Use Jenkins Credentials Manager.
* Use Docker Hub Access Token instead of password.
* Keep Jenkins and Docker versions updated.
* Use separate Jenkins agents for production workloads.

---

# 📌 Learning Outcomes

Through this project, I learned:

✅ Docker image creation
✅ Container deployment
✅ Docker Compose management
✅ Jenkins pipeline creation
✅ Jenkins agent configuration
✅ GitHub webhook automation
✅ CI/CD workflow implementation
✅ AWS EC2 based DevOps infrastructure

---

# 👨‍💻 Author

**Ali Ahmad Shah**

GitHub:

https://github.com/aliahmadshah56
