# MERN Stack Kubernetes Deployment with HELM and Jenkins CI/CD

## Overview

This project demonstrates the deployment of a **MERN (MongoDB, Express.js, React.js, Node.js)** application using Kubernetes, HELM charts, and Jenkins for CI/CD automation.

The application is split into two repositories:

- **Frontend (React)**: [learnerReportCS\_frontend](https://github.com/ranyabrkumar/Containerization_Container_Orchestration/tree/main/frontend)
- **Backend (Node.js + Express)**: [learnerReportCS\_backend](https://github.com/ranyabrkumar/Containerization_Container_Orchestration/tree/main/backend)

---

## Objectives

- **Kubernetes Deployments**: Create manifests for both frontend and backend.
- **HELM Chart**: Provide a unified deployment mechanism with configuration flexibility.
- **Jenkins Pipeline**: Automate build, push, and deployment processes.

---

## Prerequisites

- Kubernetes Cluster (Minikube, EKS, GKE, AKS, etc.)
- `kubectl` configured to access the cluster
- Docker (for image builds)
- Helm v3
- Jenkins server with:
  - Docker access
  - Kubernetes CLI tools
  - Credentials for Docker registry & kubeconfig

---

## Project Structure

```
.
├── k8s/                   # Raw Kubernetes manifests
│   ├── backend-deployment.yaml
│   ├── backend-service.yaml
│   ├── frontend-deployment.yaml
│   ├── frontend-service.yaml
│   
├── helm/
│   └── learnerreport/     # HELM chart for unified deployment
│       ├── Chart.yaml
│       ├── values.yaml
│       └── templates/
├── pipeline.jfl        # Groovy pipeline for CI/CD
└── README.md
```

---

## Deployment Workflow

### 1. Docker Image Build

Jenkins builds images for both frontend and backend:

```bash
docker build -t <registry>/learner-frontend:<tag> ./frontend
docker build -t <registry>/learner-backend:<tag> ./backend
docker push <registry>/learner-frontend:<tag>
docker push <registry>/learner-backend:<tag>
```

### 2. Deploy with Helm

Install/upgrade the application in Kubernetes:

```bash
helm create learnerreport
helm install backend ./learnerreport --values ./learnerreport/values_be.yaml
helm install frontend ./learnerreport --values ./learnerreport/values_fe.yaml
helm install mongo ./learnerreport --values ./learnerreport/values_db.yaml

```

---

## Jenkins Pipeline Overview

Stages in `Jenkinsfile`:

1.**Cleanup Workspace** — Clone FE & BE repositories.
2. **Checkout** — Clone git repositories.
3. **Build Backend Docker Image** — build and push docker image for backend.
4. **Build Frontend Docker Image** — build and push docker image for frontend.
5. **Set kubeconfig** — set kube config.
6. **Deploy using Helm** - Use Helm to update the deployment.

---

## Verification

- **Frontend**: Access frontend`.
- **Backend**: Test health endpoint (`/api/health`).
- **Logs**: `kubectl logs <pod-name>`

---

