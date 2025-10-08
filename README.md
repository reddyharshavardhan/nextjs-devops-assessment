# Next.js DevOps Assessment

A production-ready containerized Next.js application with automated CI/CD pipeline, deployed on Kubernetes.

## 🚀 Overview

This project demonstrates modern DevOps practices including:
- **Containerization** with Docker multi-stage builds
- **Automated CI/CD** using GitHub Actions
- **Container Registry** integration with GHCR
- **Kubernetes deployment** with health checks and auto-scaling
- **Production-ready** configuration and security best practices

## 📋 Prerequisites

- **Node.js** 18+ (tested with v22.11.0)
- **Docker Desktop** (tested with v28.1.1)
- **Minikube** (tested with v1.36.0)
- **kubectl** (tested with v1.32.2)
- **Git** (tested with v2.38.1)
- **GitHub Account** with Personal Access Token (PAT)

## 🏗️ Architecture

```
┌─────────────────┐     ┌──────────────────┐     ┌─────────────────┐
│  GitHub Repo    │────▶│ GitHub Actions   │────▶│      GHCR       │
│                 │     │    (CI/CD)       │     │   (Container    │
│ • Source Code   │     │ • Build Docker   │     │    Registry)    │
│ • Dockerfile    │     │ • Push to GHCR   │     │                 │
│ • K8s Manifests │     └──────────────────┘     └────────┬────────┘
└─────────────────┘                                       │
                                                          │
                        ┌──────────────────┐              │
                        │    Minikube      │◀─────────────┘
                        │  (Kubernetes)    │
                        │                  │
                        │  ┌────────────┐  │
                        │  │ Deployment │  │
                        │  │ 3 Replicas │  │
                        │  │   Health   │  │
                        │  │   Checks   │  │
                        │  └─────┬──────┘  │
                        │        │         │
                        │  ┌─────▼──────┐  │
                        │  │  Service   │  │
                        │  │ (NodePort) │  │
                        │  └────────────┘  │
                        └──────────────────┘
```

## 🛠️ Quick Start

### Local Development

```bash
# Clone repository
git clone https://github.com/reddyharshavardhan/nextjs-devops-assessment.git
cd nextjs-devops-assessment

# Install dependencies
npm install

# Run development server
npm run dev

# Access at http://localhost:3000
```

### Docker

```bash
# Build image
docker build -t nextjs-app:local .

# Run container
docker run -p 3000:3000 nextjs-app:local

# Access at http://localhost:3000
```

### Pull from GHCR

```bash
# Pull latest image
docker pull ghcr.io/reddyharshavardhan/nextjs-devops-assessment:latest

# Run container
docker run -p 3000:3000 ghcr.io/reddyharshavardhan/nextjs-devops-assessment:latest
```

## ☸️ Kubernetes Deployment

### 1. Start Minikube

```bash
minikube start --driver=docker --cpus=2 --memory=2048
kubectl cluster-info
```

### 2. Create Image Pull Secret

```bash
kubectl create secret docker-registry ghcr-secret \
  --docker-server=ghcr.io \
  --docker-username=reddyharshavardhan \
  --docker-password=YOUR_GITHUB_PAT \
  --docker-email=reddyharshavardhan125@gmail.com
```

### 3. Deploy Application

```bash
# Apply manifests
kubectl apply -f k8s/

# Verify deployment
kubectl get all

# Watch pods
kubectl get pods -w
```

### 4. Access Application

```bash
# Get service URL
minikube service nextjs-app-service --url

# Or open in browser
minikube service nextjs-app-service
```

## 📦 Docker Configuration

### Multi-Stage Build

- **Stage 1 (deps)**: Install dependencies
- **Stage 2 (builder)**: Build Next.js application
- **Stage 3 (runner)**: Production runtime

### Features

- Alpine Linux base for minimal footprint
- Non-root user for security
- Optimized layer caching
- Production-ready configuration

## 🔄 CI/CD Pipeline

The GitHub Actions workflow automatically:

1. Triggers on push to `main` branch
2. Builds Docker image
3. Tags image with:
   - Branch name
   - Git SHA with date prefix
   - `latest` for main branch
4. Pushes to GitHub Container Registry

**Workflow file**: `.github/workflows/docker-build-push.yml`

## 📁 Project Structure

```
nextjs-devops-assessment/
├── .github/
│   └── workflows/
│       └── docker-build-push.yml    # CI/CD pipeline
├── k8s/
│   ├── deployment.yaml              # K8s deployment (3 replicas)
│   └── service.yaml                 # NodePort service
├── src/
│   └── app/                         # Next.js app directory
│       ├── globals.css
│       ├── layout.tsx
│       └── page.tsx
├── public/                          # Static assets
├── .dockerignore
├── .gitignore
├── Dockerfile                       # Multi-stage build
├── next.config.js
├── package.json
├── README.md
└── tsconfig.json
```

## 🔍 Kubernetes Resources

### Deployment

- **Replicas**: 3 for high availability
- **Resources**:
  - Memory: 256Mi limit / 128Mi request
  - CPU: 200m limit / 100m request
- **Health Checks**:
  - Liveness probe (30s initial delay)
  - Readiness probe (5s initial delay)
- **Image Pull Policy**: Always

### Service

- **Type**: NodePort
- **Port**: 80 → 3000
- **NodePort**: 30001

## 🔧 Troubleshooting

### Docker Issues

```bash
# Verify Docker is running
docker version

# Clean up resources
docker system prune -a
```

### Minikube Issues

```bash
# Restart Minikube
minikube delete
minikube start --driver=docker

# Check logs
minikube logs

# SSH into Minikube
minikube ssh
```

### Kubernetes Issues

```bash
# Check pod logs
kubectl logs -l app=nextjs-app

# Describe pod
kubectl describe pod <pod-name>

# Check deployment status
kubectl rollout status deployment/nextjs-app

# Restart deployment
kubectl rollout restart deployment/nextjs-app
```

## 📊 Monitoring

```bash
# Resource usage
kubectl top nodes
kubectl top pods

# Service details
kubectl describe service nextjs-app-service

# Recent events
kubectl get events --sort-by='.lastTimestamp'
```

## 🔐 Security Best Practices

### Docker
- Non-root user execution
- Minimal Alpine base image
- No sensitive data in layers

### Kubernetes
- Resource limits defined
- Image pull secrets
- Health checks for reliability

### CI/CD
- GitHub GITHUB_TOKEN usage
- No hardcoded credentials
- Automated updates

## 🚀 Production Considerations

For production deployment:

- Use managed Kubernetes (EKS, GKE, AKS)
- Implement Horizontal Pod Autoscaler (HPA)
- Add Ingress controller
- Set up monitoring (Prometheus/Grafana)
- Use Helm charts for configuration
- Implement SSL/TLS termination
- Add logging aggregation
- Set up alerting

## 📝 Resources

- **Repository**: [github.com/reddyharshavardhan/nextjs-devops-assessment](https://github.com/reddyharshavardhan/nextjs-devops-assessment)
- **GHCR Image**: `ghcr.io/reddyharshavardhan/nextjs-devops-assessment:latest`

## 📄 License

This project is for assessment purposes.

---

**Made with ❤️ for DevOps excellence**
