This repository provides a step-by-step guide to creating a basic "Hello World" application using Python, containerizing it with Docker, and publishing the image to both Docker Hub and GitHub Container Registry (GHCR).

Table of Contents
Prerequisites
Project Structure
Step 1: Create the Python Application
Step 2: Create the Dockerfile
Step 3: Build the Docker Image
Step 4: Run the Docker Container
Step 5: Push the Image to Docker Hub
Step 6: Push the Image to GitHub Container Registry (GHCR)
Cleaning Up
Prerequisites
Before you begin, ensure you have the following installed and configured:

Docker: Install Docker Desktop (Windows/macOS) or Docker Engine (Linux). Download Docker
Git: Required for cloning this repository (if applicable) and managing source code. Download Git
Docker Hub Account: Create a free account on Docker Hub. You'll need your username.
GitHub Account: You already have one if you're reading this on GitHub.
GitHub Personal Access Token (PAT): For pushing to GHCR, you need a PAT with the write:packages scope.
Go to GitHub Settings -> Developer settings -> Personal access tokens -> Tokens (classic).
Generate a new token (classic).
Give it a descriptive name (e.g., ghcr-push).
Select the write:packages scope.
Generate the token and copy it immediately. You won't see it again. Never commit your PAT to your repository!
Project Structure
Your project directory should look like this:

.
├── Dockerfile
├── app.py
└── README.md
Step 1: Create the Python Application
Create a simple Python script named app.py that prints "Hello, Docker World!".

app.py:

# Simple Python script to print a message
print("Hello, Docker World!")
This is the application we will containerize.

Step 2: Create the Dockerfile
Create a file named Dockerfile (no extension) in the same directory. This file contains instructions for Docker to build the image.

Dockerfile:

# Use an official Python runtime as a parent image
# Using a slim version to keep the image size small
FROM python:3.9-slim

# Set the working directory inside the container
WORKDIR /app

# Copy the current directory contents into the container at /app
COPY . /app

# Define the command to run your application
# This command runs when the container launches
CMD ["python", "./app.py"]
Explanation:

FROM python:3.9-slim: Specifies the base image to use. We're starting with a lightweight official Python 3.9 image.
WORKDIR /app: Sets the working directory inside the container to /app. Any subsequent COPY, RUN, CMD commands will execute relative to this directory.
COPY . /app: Copies the contents of the current directory (where the Dockerfile is located) into the /app directory inside the container. In this case, it copies app.py.
CMD ["python", "./app.py"]: Specifies the default command to run when a container is started from this image. It executes python ./app.py.
Step 3: Build the Docker Image
Open your terminal or command prompt, navigate to the directory containing your Dockerfile and app.py, and run the following command:

docker build -t hello-docker-world:latest .
Explanation:

docker build: The command to build an image from a Dockerfile.
-t hello-docker-world:latest: Tags the image.
hello-docker-world is the name of the image.
:latest is the tag (often representing the latest version). You can use specific versions like :1.0.
.: Specifies the build context (the current directory). Docker looks for the Dockerfile here and sends files from this directory to the Docker daemon.
After the build completes successfully, you can verify the image was created:

docker images
You should see hello-docker-world listed with the latest tag.

Step 4: Run the Docker Container
Now, run a container based on the image you just built:

docker run --rm hello-docker-world:latest
Explanation:

docker run: The command to create and start a new container from an image.
--rm: Automatically removes the container when it exits. This is useful for short-lived containers like this one to keep things clean.
hello-docker-world:latest: The name and tag of the image to use.
You should see the following output in your terminal:

Hello, Docker World!
This confirms your application is running correctly inside the Docker container.

Step 5: Push the Image to Docker Hub
To share your image or use it elsewhere, you can push it to a container registry like Docker Hub.

Tag the image for Docker Hub: You need to tag your image with your Docker Hub username. Replace your-dockerhub-username with your actual Docker Hub username.

docker tag hello-docker-world:latest your-dockerhub-username/hello-docker-world:latest
# You can also add a specific version tag
# docker tag hello-docker-world:latest your-dockerhub-username/hello-docker-world:1.0
Log in to Docker Hub: Use the docker login command. You'll be prompted for your Docker Hub username and password (or an access token if you have 2FA enabled).

docker login
Push the image: Push the tagged image to Docker Hub.

docker push your-dockerhub-username/hello-docker-world:latest
# If you tagged with a version:
# docker push your-dockerhub-username/hello-docker-world:1.0
After the push completes, your image will be available on your Docker Hub profile (e.g., https://hub.docker.com/r/your-dockerhub-username/hello-docker-world).

Step 6: Push the Image to GitHub Container Registry (GHCR)
You can also push your image to GitHub's own container registry.

Tag the image for GHCR: Tag your image using the GHCR format: ghcr.io/YOUR_GITHUB_USERNAME/IMAGE_NAME:TAG. Replace your-github-username with your lowercase GitHub username. The image name can be the same as your repository name or something else like hello-docker-world.

# Make sure to use your LOWERCASE GitHub username
docker tag hello-docker-world:latest ghcr.io/your-github-username/hello-docker-world:latest
# Example using repo name (replace 'your-repo-name'):
# docker tag hello-docker-world:latest ghcr.io/your-github-username/your-repo-name:latest
Log in to GHCR: Use docker login with the GHCR server address. Use your GitHub username and the Personal Access Token (PAT) you created in the prerequisites as the password.

# Replace YOUR_GITHUB_USERNAME with your actual GitHub username (case might matter here, try lowercase first)
docker login ghcr.io -u YOUR_GITHUB_USERNAME
Username: Enter your GitHub username.
Password: Paste your GitHub Personal Access Token (PAT) with write:packages scope.
Security Note: For automated environments (like GitHub Actions), it's better to pipe the token via stdin: echo $CR_PAT | docker login ghcr.io -u YOUR_GITHUB_USERNAME --password-stdin (where CR_PAT is an environment variable holding your PAT).

Push the image: Push the tagged image to GHCR. Ensure the image name matches the one you used in the docker tag command.

docker push ghcr.io/your-github-username/hello-docker-world:latest
# Or if you used your repo name:
# docker push ghcr.io/your-github-username/your-repo-name:latest
After the push completes, your image will be available in the "Packages" section of your GitHub profile or repository. The image will likely be private by default unless your repository is public and you adjust package visibility settings.

Cleaning Up
To remove the container (if you didn't use --rm), the image, and log out:

# List running/stopped containers (optional)
docker ps -a

# Stop a specific container (if needed, replace <container_id>)
# docker stop <container_id>

# Remove a specific container (if needed, replace <container_id>)
# docker rm <container_id>

# Remove the built images (use the names/tags you created)
docker rmi hello-docker-world:latest
docker rmi your-dockerhub-username/hello-docker-world:latest  # If tagged for Docker Hub
docker rmi ghcr.io/your-github-username/hello-docker-world:latest # If tagged for GHCR (use actual name/tag)

# Log out from registries (optional)
# docker logout
# docker logout ghcr.io
Happy Dockering!
