\# Next.js DevOps Assessment



This project demonstrates containerizing and deploying a Next.js application using Docker, GitHub Actions, and Kubernetes (Minikube).



\## Prerequisites



\- Docker Desktop

\- Minikube

\- kubectl

\- Node.js 18+

\- GitHub account



\## Setup Instructions



\### 1. Local Development



Install dependencies:

npm install



Run development server:

npm run dev



\### 2. Build Docker Image Locally



Build the Docker image:

docker build -t nextjs-app:local .



Run the container locally:

docker run -p 3000:3000 nextjs-app:local



\### 3. Setup GitHub Container Registry



1\. Fork/Clone this repository

2\. Go to Settings > Secrets and variables > Actions

3\. The GITHUB\_TOKEN is automatically available



\### 4. Deploy to Minikube



Start Minikube:

minikube start



Create secret for pulling from GHCR (replace with your values):

kubectl create secret docker-registry ghcr-secret --docker-server=ghcr.io --docker-username=reddyharshavardhan --docker-password=YOUR\_GITHUB\_PAT --docker-email=YOUR\_EMAIL



Apply Kubernetes manifests:

kubectl apply -f k8s/



Check deployment status:

kubectl get deployments

kubectl get pods

kubectl get services



Get Minikube service URL:

minikube service nextjs-app-service --url



\## Accessing the Application



\### Local Docker

\- http://localhost:3000



\### Minikube

\- Run minikube service nextjs-app-service --url to get the URL

\- Or access via http://$(minikube ip):30001



\## GitHub Actions Workflow



The CI/CD pipeline automatically:

1\. Builds Docker image on push to main branch

2\. Tags images with branch name, SHA, and date

3\. Pushes to GitHub Container Registry (ghcr.io)



\## Image URLs



\- GHCR Image: ghcr.io/reddyharshavardhan/nextjs-devops-assessment:latest



\## Architecture



\- Next.js: Frontend framework with server-side rendering

\- Docker: Multi-stage build for optimized production images

\- GitHub Actions: CI/CD pipeline for automated builds

\- Kubernetes: Orchestration with health checks and 3 replicas

\- Minikube: Local Kubernetes cluster for development



\## Best Practices Implemented



1\. Docker Optimization:

&nbsp;  - Multi-stage builds

&nbsp;  - Non-root user

&nbsp;  - Minimal base image (Alpine)

&nbsp;  - Layer caching



2\. Kubernetes:

&nbsp;  - Health checks (liveness \& readiness probes)

&nbsp;  - Resource limits

&nbsp;  - Multiple replicas for HA



3\. CI/CD:

&nbsp;  - Automated builds on push

&nbsp;  - Proper image tagging

&nbsp;  - Secure registry authentication

