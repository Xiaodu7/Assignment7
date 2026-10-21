# IS601 Module 7 - Dockerized QR Code Generator

## Project Overview

This project is a QR Code Generator application written in Python. It takes a URL as input and generates a QR code image in PNG format.

For Module 7, I used Docker to containerize the application so it can run in an isolated environment without manually installing Python dependencies on the host computer.

## Technologies Used

- Python 3.12
- Docker and Docker Compose
- Git and GitHub
- DockerHub
- GitHub Actions

## Project Files

- `main.py` - Main Python program for generating QR codes.
- `Dockerfile` - Instructions for building the Docker image.
- `docker-compose.yml` - Configuration for running the application with Docker Compose.
- `requirements.txt` - Python dependencies.
- `.dockerignore` - Files excluded from the Docker build context.
- `.gitignore` - Files excluded from Git tracking.
- `.github/workflows/docker-ci.yml` - GitHub Actions workflow for automated building and testing.

## How to Run the Application

### 1. Build the Docker Image

Make sure Docker Desktop is running, then open a terminal in the project directory.

Run:

`docker build -t qr-code-generator-app .`

This command creates a Docker image using the provided Dockerfile.

### 2. Run the Docker Container

Run:

`docker run --name qr-generator qr-code-generator-app --url http://www.njit.edu`

This command generates a QR code for the NJIT website.

The QR code is saved inside the container at `/app/qr_codes`.

To check the application logs, run:

`docker logs qr-generator`

### 3. Save QR Codes to the Local Folder

To save the generated QR code on the local computer, first create a directory:

`mkdir -p qr_codes`

Then run:

`docker run --rm --name qr-generator-volume -v "$(pwd)/qr_codes:/app/qr_codes" qr-code-generator-app --url http://www.njit.edu`

The generated PNG file will be saved in the local `qr_codes` folder.

### 4. Run with Docker Compose

The project also supports Docker Compose.

Run:

`docker compose up --build`

Docker Compose automatically builds and runs the QR Code Generator using the configuration in `docker-compose.yml`.

The configuration uses these environment variables:

- `QR_CODE_DIR=/app/qr_codes` - Output directory for QR codes.
- `FILL_COLOR=red` - QR code color.
- `BACK_COLOR=white` - Background color.

The local `qr_codes` folder is mounted to the container so the generated files can be accessed outside Docker.

## Docker Security

The Dockerfile uses a lightweight Python base image and creates a non-root user called `myuser`.

The application runs as this user, and the required directories have appropriate permissions. This helps improve container security.

## GitHub Actions

This project includes a GitHub Actions workflow for Continuous Integration (CI).

The workflow automatically:

1. Checks out the repository.
2. Builds the Docker image.
3. Runs the QR Code Generator.
4. Verifies that a PNG QR code file was generated.

The GitHub Actions workflow has been tested successfully.

## DockerHub

The Docker image has been uploaded to DockerHub.

**DockerHub Image:**

https://hub.docker.com/r/xdu7/qr-code-generator-app

To download the image, run:

`docker pull xdu7/qr-code-generator-app:latest`

## GitHub Repository

The project source code and GitHub Actions workflow are available at:

https://github.com/Xiaodu7/Assignment7

## Conclusion

Through this assignment, I learned how to build and run Docker containers, use Docker Compose, configure volume mounts, and push Docker images to DockerHub. I also learned how to use GitHub Actions to automate the building and testing process.