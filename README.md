# Flask MySQL Connector – Dockerized

A Dockerized version of the Flask MySQL Connector project, built for learning and DevOps practice.

> **Note:** The original project was created by its original author. This repository is a personal learning project where Docker support was added to containerize the application.

---

## Features

- Dockerized Python application
- Lightweight Docker image using `python:3.9-slim`
- Dependency management with `requirements.txt`
- Clean Docker build using `--no-cache-dir`
- Ready for local development and DevOps practice

---

## Project Structure

```text
.
├── Dockerfile
├── .dockerignore
├── requirements.txt
├── setup.py
├── setup.cfg
├── flask_mysql_connector/
├── examples/
├── README.md
└── LICENSE
```

---

## Prerequisites

- Docker Desktop or Docker Engine
- Git

---

## Clone Repository

```bash
git clone https://github.com/aliahmadshah56/flask_mysql_connector.git
cd flask_mysql_connector
```

---

## Build Docker Image

```bash
docker build -t flask_mysql_connector .
```

## Verify Image

```bash
docker images
```

## Run Container

```bash
docker run --rm flask_mysql_connector
```

---

## Docker Concepts Practiced

- Writing a Dockerfile
- Creating a lightweight Python image
- Using a working directory
- Copying project files
- Installing dependencies from `requirements.txt`
- Using `pip install --no-cache-dir`
- Building Docker images
- Running Docker containers
- Using `.dockerignore`

---

## Technologies Used

- Python 3.9
- Flask
- MySQL Connector Python
- Pandas
- Docker

---

## Learning Objective

This repository was created to practice Docker by containerizing an existing Python project. The primary goal was to understand how to:

- Create Docker images
- Optimize image size
- Manage Python dependencies
- Build reproducible development environments

---

## License

This project follows the license included in the original repository.
