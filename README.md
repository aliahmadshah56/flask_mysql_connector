# Flask Application CI/CD Pipeline with Jenkins, Docker & Shared Libraries

A DevOps project demonstrating an end-to-end CI/CD pipeline: source code is pulled from GitHub, built into a Docker image on a Jenkins agent, pushed to DockerHub, and deployed with Docker Compose. Pipeline logic is modularized using a Jenkins Shared Library.

## Tech Stack

| Tool | Purpose |
|---|---|
| GitHub | Source code management, webhook trigger |
| Jenkins | CI/CD orchestration |
| Jenkins Agent | Isolated build/execution node |
| Docker | Application containerization |
| Docker Compose | Multi-container orchestration |
| DockerHub | Image registry |
| Groovy | Jenkins pipeline scripting |

## Architecture

```
Developer → git push → GitHub
                          |
                    (webhook trigger)
                          ↓
                  Jenkins (Controller)
                          |
                          ↓
                    Jenkins Agent
                          |
                  docker build → docker push
                          |
                       DockerHub
                          |
                    docker compose up
                          ↓
                  Running Container
```
<img width="1266" height="875" alt="image" src="https://github.com/user-attachments/assets/1e70698c-7974-43e7-81b9-36004818c33c" />
<img width="1272" height="782" alt="image" src="https://github.com/user-attachments/assets/0fd2a256-6f65-4685-a554-24925939078c" />

## Project Structure


```
.
├── Dockerfile
├── docker-compose.yml
├── Jenkinsfile
├── requirements.txt
├── flask_mysql_connector/
└── README.md
```

## Prerequisites

- Ubuntu server(s) for Jenkins controller and agent (EC2 or equivalent)
- Jenkins installed on the controller
- Docker and Docker Compose installed on the agent
- Git
- A DockerHub account

## Docker Setup

**Dockerfile** — builds the Python environment and installs dependencies:

```dockerfile
FROM python:3.9-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
CMD ["python", "app.py"]
```

**docker-compose.yml** — defines the application service:

```yaml
services:
  flask:
    container_name: flask
    build:
      context: .
    ports:
      - "5000:5000"
```

## Jenkins Pipeline Stages

1. **Clone** — Jenkins pulls the latest code from GitHub.
2. **Build** — builds the Docker image.
   ```bash
   docker build -t flask-app:latest .
   ```
3. **Test** — runs application checks (placeholder for future test integration).
4. **Push** — tags and pushes the image to DockerHub.
   ```bash
   docker login
   docker tag flask-app:latest <dockerhub-username>/flask-app:latest
   docker push <dockerhub-username>/flask-app:latest
   ```
5. **Deploy** — recreates the container via Docker Compose.
   ```bash
   docker compose down
   docker compose up --build -d
   ```

## Jenkins Configuration

**DockerHub credentials:**
Jenkins Dashboard → Manage Jenkins → Credentials → Add Credentials
- Kind: `Username with password`
- Username: your DockerHub username
- Password: DockerHub password or access token
- ID: `dockerhub-cred`

**Jenkins Agent setup:**
```bash
sudo usermod -aG docker ubuntu
exit          # re-login for group change to take effect
docker ps     # verify agent can run docker commands
```

## Jenkins Shared Library

Pipeline steps are modularized into a shared library for reuse across projects:

```
Jenkins-Shared-Library
└── vars
    ├── docker_build.groovy
    ├── docker_push.groovy
    └── gitclone.groovy
```

Usage in the Jenkinsfile:

```groovy
@Library('shared') _

docker_build(
  imageName: "flask-app",
  imageTag: "latest"
)
```

## Troubleshooting

**Docker permission denied**
```
permission denied while trying to connect to the Docker daemon socket
```
Fix: add the Jenkins agent user to the `docker` group and restart the agent session.
```bash
sudo usermod -aG docker ubuntu
```

**DockerHub push access denied**
Make sure the image is tagged with your DockerHub namespace before pushing.
```bash
# Wrong
docker push flask-app:latest

# Correct
docker tag flask-app:latest <dockerhub-username>/flask-app:latest
docker push <dockerhub-username>/flask-app:latest
```

**Jenkins agent goes offline / build fails on disk space**
```bash
df -h
docker system prune -a
```

## What This Project Covers

- Declarative Jenkins pipelines
- Jenkins agent configuration
- Docker image build and DockerHub push
- Docker Compose–based deployment
- GitHub webhook–triggered builds
- Jenkins Shared Libraries for reusable pipeline code

## Author

**Ali Ahmad Shah**
GitHub: [aliahmadshah56](https://github.com/aliahmadshah56)
DockerHub: [aliahmadshah](https://hub.docker.com/u/aliahmadshah)
