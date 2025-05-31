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
