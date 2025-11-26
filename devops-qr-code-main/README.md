# devops-qr-code

This is the sample application for the DevOps Capstone Project.
It generates QR Codes for the provided URL, the front-end is in NextJS and the API is written in Python using FastAPI.

## Application

**Front-End** - A web application where users can submit URLs.

**API**: API that receives URLs and generates QR codes. The API stores the QR codes in cloud storage(AWS S3 Bucket).

## Running locally

### API

The API code exists in the `api` directory. You can run the API server locally:

- Clone this repo
- Make sure you are in the `api` directory
- Create a virtualenv by typing in the following command: `python -m venv .venv`
- Install the required packages: `pip install -r requirements.txt`
- Create a `.env` file, and add you AWS Access and Secret key, check  `.env.example`
- Also, change the BUCKET_NAME to your S3 bucket name in `main.py`
- Run the API server: `uvicorn main:app --reload`
- Your API Server should be running on port `http://localhost:8000`

### Front-end

The front-end code exits in the `front-end-nextjs` directory. You can run the front-end server locally:

- Clone this repo
- Make sure you are in the `front-end-nextjs` directory
- Install the dependencies: `npm install`
- Run the NextJS Server: `npm run dev`
- Your Front-end Server should be running on `http://localhost:3000`


## Goal

The goal is to get hands-on with DevOps practices like Containerization, CICD and monitoring.

Look at the capstone project for more detials.

## What's Added After Forking

This repository has been extended with the following DevOps implementations:

### 🐳 Dockerization

Both the API and front-end have been containerized for easy deployment:

#### API Dockerfile (`api/Dockerfile`)
- Base image: `python:3.9`
- Installs Python dependencies from `requirements.txt`
- Exposes port 8000 for the FastAPI server
- Runs with uvicorn on `0.0.0.0:8000`

#### Front-end Dockerfile (`front-end-nextjs/Dockerfile`)
- Base image: `node:18-alpine`
- Multi-stage build for optimized image size
- Supports multiple package managers (npm, yarn, pnpm)
- Builds the Next.js application
- Exposes port 3000
- Runs with `npm start`

**Running with Docker:**

```bash
# Build and run API
cd api
docker build -t devops-qr-code-api .
docker run -p 8000:8000 --env-file .env devops-qr-code-api

# Build and run Front-end
cd front-end-nextjs
docker build -t devops-qr-code-frontend .
docker run -p 3000:3000 devops-qr-code-frontend
```

### 🚀 CI/CD Pipeline

Automated build and deployment pipeline using GitHub Actions (`.github/workflows/build-docker.yaml`):

**Workflow Features:**
- **Trigger**: Automatically runs on push to `main` branch when Dockerfiles are modified
- **Jobs**: 
  - Checks out the repository
  - Builds Docker images for both API and front-end
  - Pushes images to Docker Hub:
    - `ankitrj3/devops-qr-code-api:latest`
    - `ankitrj3/devops-qr-code-frontend:latest`

**Pre-requisites for CI/CD:**
- Docker Hub account
- `DOCKER_HUB_TOKEN` secret configured in GitHub repository settings

**Pull and run published images:**

```bash
# Pull and run API
docker pull ankitrj3/devops-qr-code-api:latest
docker run -p 8000:8000 --env-file .env ankitrj3/devops-qr-code-api:latest

# Pull and run Front-end
docker pull ankitrj3/devops-qr-code-frontend:latest
docker run -p 3000:3000 ankitrj3/devops-qr-code-frontend:latest
```

## Original Author

[Rishab Kumar](https://github.com/rishabkumar7)

## Forked & Extended By

[Likhitha sri](https://github.com/Ankitrj3)

## License

[MIT](./LICENSE)
