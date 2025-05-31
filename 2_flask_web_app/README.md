# Simple Flask Web App - Docker Image #2


This repository provides a step-by-step guide to creating a basic Flask web application, containerizing it using Docker, and running it locally. It is part of the **100 Docker Images from Basic to Advanced** series designed for interview preparation and production use.

## Table of Contents

1. [Prerequisites](#prerequisites)
2. [Project Structure](#project-structure)
3. [Step 1: Create the Flask Application](#step-1-create-the-flask-application)
4. [Step 2: Define the Requirements](#step-2-define-the-requirements)
5. [Step 3: Create the Dockerfile](#step-3-create-the-dockerfile)
6. [Step 4: Build the Docker Image](#step-4-build-the-docker-image)
7. [Step 5: Run the Docker Container](#step-5-run-the-docker-container)
8. [Step 6: Access the Web App](#step-6-access-the-web-app)
9. [Step 7: Push the Image to Docker Hub](#step-7-push-the-image-to-docker-hub)
10. [Cleaning Up](#cleaning-up)

---

## Prerequisites

Before starting, make sure the following tools are installed on your system:

- **Docker:** [Download Docker](https://www.docker.com/get-started)
- **Git (optional):** [Download Git](https://git-scm.com/)
- **Docker Hub Account:** [Create one here](https://hub.docker.com/)

---

## Project Structure

Your project directory should look like this:

```
flask-hello-docker/
├── Dockerfile
├── app.py
├── requirements.txt
└── README.md
```

---

## Step 1: Create the Flask Application

Let’s create a basic Flask web app that returns a message at the root URL (`/`).

**`app.py`**:
```python
from flask import Flask

app = Flask(__name__)

@app.route('/')
def hello():
    return "Hello from Flask in Docker!"

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```

**Explanation:**

- We define a route `/` which returns a simple string.
- `host='0.0.0.0'` tells Flask to listen on all available network interfaces.
- `port=5000` is the default Flask port.

---

## Step 2: Define the Requirements

Flask needs to be installed in the container, so we define it in a `requirements.txt` file.

**`requirements.txt`**:
```
flask==2.3.2
```

This ensures consistency across environments.

---

## Step 3: Create the Dockerfile

The `Dockerfile` tells Docker how to build the image.

**`Dockerfile`**:
```dockerfile
# Use official Python slim image
FROM python:3.9-slim

# Set working directory
WORKDIR /app

# Copy requirements and install
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy app code
COPY . .

# Expose port used by Flask
EXPOSE 5000

# Run the app
CMD ["python", "app.py"]
```

**Explanation:**

- We use `python:3.9-slim` to keep image size minimal.
- `COPY requirements.txt` and `RUN pip install` to install dependencies.
- `CMD` specifies the command to start the app.

---

## Step 4: Build the Docker Image

Navigate to your project directory and run:

```bash
docker build -t flask-hello-docker:latest .
```

**Explanation:**

- `-t flask-hello-docker:latest`: Names the image and tags it as `latest`.
- `.`: Uses the current directory as the build context.

---

## Step 5: Run the Docker Container

Run the container and map the internal port 5000 to your machine:

```bash
docker run -d -p 5000:5000 flask-hello-docker
```

**Explanation:**

- `-d`: Detached mode (runs in background).
- `-p 5000:5000`: Maps container port 5000 to your local machine.

---

## Step 6: Access the Web App

Open your browser and visit:  
[http://localhost:5000](http://localhost:5000)

You should see:

```
Hello from Flask in Docker!
```

---

## Step 7: Push the Image to Docker Hub

### 1. Tag the image:

```bash
docker tag flask-hello-docker your-dockerhub-username/flask-hello-docker:latest
```

### 2. Log in to Docker Hub:

```bash
docker login
```

### 3. Push the image:

```bash
docker push your-dockerhub-username/flask-hello-docker:latest
```

Once pushed, the image will be available at:

```
https://hub.docker.com/r/your-dockerhub-username/flask-hello-docker
```

---

## Cleaning Up

To stop and remove the container and image:

```bash
# Stop all running containers
docker ps -q --filter ancestor=flask-hello-docker | xargs docker stop

# Remove image
docker rmi flask-hello-docker
```

---

## Next Steps

In the next image (#3), we’ll cover using **environment variables** and **volumes** in a containerized Flask app.

---

Happy Dockering!
