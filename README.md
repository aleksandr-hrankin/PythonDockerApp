# Hello World Python Application with Docker and AWS ECR

This Python application prints the string 'Hello World' and is designed to demonstrate how to containerize an application using Docker and deploy it on AWS ECR.

## Create APP and Docker Image

1) Create an application, for example `main.py`.
2) Create a Dockerfile.

Use this small Python Docker image for a base:
https://github.com/Docker-Hub-frolvlad/docker-alpine-python3/

Here's the content of the Dockerfile:

```Dockerfile
# Use the official Python image as a base
FROM python:3-alpine

# Set the working directory in the container
WORKDIR ./

# Copy the dependency files into the working directory
COPY requirements.txt ./

# Install dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Command to run the application
CMD ["python", "./main.py"]
```

3) Run the Docker engine.
4) From the app folder, build the Docker image:
   ```bash
   docker build -t your_image_name .
   ```
5) To test, run:
   ```bash
   docker run -p 4000:80 your_image_name
   ```

## Push to AWS ECR

1) Download and install the AWS CLI.
2) Configure AWS CLI:
   ```bash
   aws configure
   ```
3) Create an ECR repository:
   ```bash
   aws ecr create-repository --repository-name your_repository_name
   ```
4) Authenticate Docker with ECR:
   ```bash
   aws ecr get-login-password --region your_region | docker login --username AWS --password-stdin your_aws_account_id.dkr.ecr.your_region.amazonaws.com
   ```
5) Tag your Docker image:
   ```bash
   docker tag your_image_name:tag your_aws_account_id.dkr.ecr.your_region.amazonaws.com/your_repository_name:tag
   ```
6) Push the Docker image to ECR:
   ```bash
   docker push your_aws_account_id.dkr.ecr.your_region.amazonaws.com/your_repository_name:tag
   ```

Replace `your_image_name`, `your_repository_name`, `your_region`, and `your_aws_account_id` with your actual AWS and Docker details.
