# 🛒 Retail Store Microservices — DevOps Pipeline (Version 1 - Local Setup)

> A production-grade DevOps project implementing a **5-service retail store e-commerce application** built entirely on local infrastructure — zero cloud cost. Covers Docker, Kubernetes, Helm, Terraform, CI/CD, and Observability using industry-standard open-source tools.

---

## 🏗️ Architecture Overview

```
                    ┌──────────────────────────────────────────┐
                    │          Retail Store Application         │
                    │         http://localhost:8080             │
                    └──────────────────┬───────────────────────┘
                                       │
                    ┌──────────────────▼───────────────────────┐
                    │              UI Service                    │
                    │           (Java Spring Boot)               │
                    └────┬──────────────┬──────────────┬────────┘
                         │              │               │
           ┌─────────────▼──┐   ┌───────▼──────┐  ┌───▼──────────────┐
           │ Catalog Service │   │ Cart Service  │  │  Orders Service  │
           │   (Go + MySQL)  │   │(Spring Boot + │  │ (Spring Boot +   │
           │                 │   │   DynamoDB)   │  │  PostgreSQL)     │
           └─────────────────┘   └──────────────┘  └────────┬─────────┘
                                                             │
                                             ┌───────────────▼──────────┐
                                             │     Checkout Service      │
                                             │  (Node.js + Redis +       │
                                             │      RabbitMQ)            │
                                             └──────────────────────────┘
```

---

## 🧰 Tech Stack

| Category | Technology | Purpose |
|---|---|---|
| **Containerization** | Docker, Docker Compose | Build and run containers |
| **Advanced Builds** | Docker BuildKit / buildx | Multi-platform image builds |
| **Orchestration** | Kubernetes (Minikube) | Container orchestration |
| **Package Manager** | Helm | K8s application management |
| **IaC** | Terraform | Infrastructure as Code |
| **CI** | GitHub Actions | Automated build and push |
| **CD** | ArgoCD | GitOps deployments |
| **Tracing** | Jaeger + OpenTelemetry | Distributed tracing |
| **Metrics** | Prometheus + Grafana | Dashboards and alerting |
| **Logging** | Grafana Loki | Log aggregation |
| **Image Registry** | Docker Hub | Container image storage |
| **Languages** | Java (Spring Boot), Node.js, Go | Microservice backends |

---

## ☁️ AWS → Local Mapping

This project replicates a full AWS DevOps setup using free, open-source alternatives:

| AWS Service | Local Equivalent Used |
|---|---|
| AWS EKS | Minikube |
| Amazon RDS MySQL | MySQL Docker container |
| Amazon RDS PostgreSQL | PostgreSQL Docker container |
| Amazon ElastiCache Redis | Redis Docker container |
| Amazon SQS | RabbitMQ Docker container |
| Amazon DynamoDB | DynamoDB Local (Docker) |
| AWS ECR | Docker Hub (free) |
| AWS ALB + Ingress | Nginx Ingress (Minikube addon) |
| AWS CloudWatch Logs | Grafana Loki |
| AWS X-Ray | Jaeger |
| Amazon Managed Prometheus | Self-hosted Prometheus |
| Amazon Managed Grafana | Self-hosted Grafana |

---

## 📁 Project Structure

```
retail-store-devops-pipeline/
│
├── 01-docker-basics/           # Docker CLI, container lifecycle
├── 02-dockerfiles/             # Custom images, multi-stage builds
├── 03-docker-compose/          # Multi-container orchestration
├── 04-docker-buildkit/         # BuildKit, multi-platform builds
├── 05-terraform-basics/        # IaC, VPC, state management
├── 06-kubernetes/
│   ├── pods/                   # Pod definitions
│   ├── deployments/            # Deployment manifests
│   ├── services/               # ClusterIP, NodePort, LoadBalancer
│   ├── configmaps/             # Environment config
│   ├── secrets/                # Secrets management
│   ├── storage/                # PV, PVC, StorageClass
│   ├── ingress/                # Nginx ingress rules
│   └── hpa/                    # Horizontal Pod Autoscaler
├── 07-helm/                    # Helm charts for all services
├── 08-observability/
│   ├── prometheus/             # Metrics collection config
│   ├── grafana/                # Dashboards and alerts
│   └── opentelemetry/          # Tracing with Jaeger
├── 09-cicd/
│   ├── github-actions/         # CI workflows (.github/workflows/)
│   └── argocd/                 # ArgoCD app definitions
└── 10-final-project/
    ├── microservices/
    │   ├── ui-service/
    │   ├── catalog-service/
    │   ├── cart-service/
    │   ├── orders-service/
    │   └── checkout-service/
    ├── docker-compose.yml      # Full local stack
    ├── kubernetes/             # All K8s manifests
    └── helm/                   # Helm chart for full app
```

---

## 🚀 Microservices

| Service | Language | Database | Local Port |
|---|---|---|---|
| **UI Service** | Java Spring Boot | — | 8080 |
| **Catalog Service** | Go | MySQL | 8081 |
| **Cart Service** | Java Spring Boot | DynamoDB Local | 8082 |
| **Orders Service** | Java Spring Boot | PostgreSQL | 8083 |
| **Checkout Service** | Node.js | Redis + RabbitMQ | 8084 |

---

## ⚡ Quick Start

### Prerequisites

```bash
# Tools needed (all free)
Docker Desktop    → https://www.docker.com/products/docker-desktop
Minikube          → https://minikube.sigs.k8s.io
kubectl           → included with Docker Desktop
Helm              → https://helm.sh
Terraform         → https://terraform.io
Git               → https://git-scm.com
VS Code           → https://code.visualstudio.com
```

### Run Full Stack with Docker Compose

```bash
# Clone the repo
git clone https://github.com/Nikhil-Singh-24/retail-store-devops-pipeline.git
cd retail-store-devops-pipeline

# Start all 5 services
cd 10-final-project
docker-compose up -d

# Check all containers running
docker ps

# Access the app
open http://localhost:8080
```

### Run on Kubernetes (Minikube)

```bash
# Start local Kubernetes cluster
minikube start --memory=8192 --cpus=4

# Enable ingress addon
minikube addons enable ingress

# Deploy all services
kubectl apply -f 10-final-project/kubernetes/

# Check pods are running
kubectl get pods

# Get app URL
minikube service ui-service --url
```

---

## 🔄 CI/CD Pipeline

```
Developer pushes code
        │
        ▼
GitHub Actions (CI)
  - Build Docker image
  - Run tests
  - Push to Docker Hub
        │
        ▼
Update Helm values.yaml
  (new image tag)
        │
        ▼
ArgoCD detects change (CD)
  - Auto-sync enabled
  - Pulls latest image
  - Deploys to Kubernetes
  - Self-heals on drift
        │
        ▼
Live on Minikube ✅
```

---

## 📊 Observability Stack

| Tool | Purpose | Local URL |
|---|---|---|
| **Prometheus** | Metrics scraping | http://localhost:9090 |
| **Grafana** | Dashboards & visualization | http://localhost:3000 |
| **Jaeger** | Distributed tracing | http://localhost:16686 |
| **Loki** | Log aggregation | via Grafana |

---

## 🗂️ Section Progress

| # | Section | Topics | Status |
|---|---|---|---|
| 01 | Docker Basics | CLI, container lifecycle, image cache | 🔄 In Progress |
| 02 | Dockerfile Mastery | FROM, RUN, COPY, multi-stage builds | ⏳ Pending |
| 03 | Docker Compose | Multi-container, volumes, networks | ⏳ Pending |
| 04 | Docker BuildKit | buildx, multi-platform images | ⏳ Pending |
| 05 | Terraform Basics | Providers, variables, state, modules | ⏳ Pending |
| 06 | Kubernetes | Pods, Deployments, Services, Secrets | ⏳ Pending |
| 07 | Helm | Charts, templates, values, releases | ⏳ Pending |
| 08 | Observability | Prometheus, Grafana, OpenTelemetry | ⏳ Pending |
| 09 | CI/CD | GitHub Actions + ArgoCD GitOps | ⏳ Pending |
| 10 | Final Project | Full 5-service integrated deployment | ⏳ Pending |

---

## 📚 Key Concepts Covered

- Docker image layering, build cache, and multi-stage optimization
- Container lifecycle — create, start, stop, remove
- Multi-container orchestration with Docker Compose health checks and dependencies
- Multi-platform image builds for AMD64 and ARM64
- Kubernetes Pods, ReplicaSets, Deployments, and Services (all 5 types)
- ConfigMaps and Secrets for environment configuration
- Persistent Volumes (PV), Persistent Volume Claims (PVC), and StorageClasses
- Kubernetes Ingress with Nginx and TLS termination
- Horizontal Pod Autoscaler (HPA) with CPU and memory metrics
- Helm chart creation, templating, versioning, and publishing
- Terraform providers, resources, variables, outputs, and modules
- Remote backend state management (S3 + DynamoDB → simulated locally)
- GitOps workflow — code commit triggers automated deployment via ArgoCD
- Distributed tracing with OpenTelemetry auto-instrumentation
- Metrics collection and Grafana dashboard creation

---

## 🛠️ Local Tools Setup Guide

```bash
# Verify all tools installed correctly
docker version
docker-compose version
minikube version
kubectl version --client
helm version
terraform version
git --version
```

---

## 👤 Author

**Nikhil Kumar**
BTech — University of Petroleum and Energy Studies (UPES)
📍 Dehradun, Uttarakhand, India
🐙 GitHub: [Nikhil-Singh-24](https://github.com/Nikhil-Singh-24)

---

## 📄 Note

This is a **Version 1 local implementation** of a production-grade AWS DevOps project.
All AWS-managed services have been replaced with equivalent open-source Docker containers and local Kubernetes.
The architecture, manifests, Helm charts, Terraform configs, and CI/CD pipelines are production-equivalent and demonstrate the same DevOps skills used in real enterprise environments.

---

