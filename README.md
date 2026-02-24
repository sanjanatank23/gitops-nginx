# GitOps CI/CD with ArgoCD (Local Setup)

## Overview

This project demonstrates a **complete local GitOps workflow** using Kubernetes, ArgoCD, Docker, and GitHub Actions.
The goal is to automate application deployment so that **Git becomes the single source of truth** and deployments happen automatically without manual `kubectl apply`.

The setup runs entirely on **local Kubernetes (Kind)** and follows real-world DevOps practices.

---

## Architecture Flow

```
Code Change
   ↓
GitHub Actions (CI)
   ↓
Docker Image Build & Push
   ↓
GitOps Repo Update (K8s manifests)
   ↓
ArgoCD Auto Sync
   ↓
Kubernetes Deployment
   ↓
Application Access (Port-forward / Ingress)
```

---

## Tools Used

* Docker
* Kubernetes (Kind – local cluster)
* ArgoCD (GitOps CD tool)
* GitHub Actions (CI pipeline)
* Docker Hub (Image registry)

---

## Repository Structure

```
gitops-nginx/
├── my-app/
│   ├── Dockerfile
│   └── index.html
├── manifests/
│   ├── deployment.yaml
│   ├── service.yaml
│   ├── ingress.yaml
│   └── application.yaml
└── .github/
    └── workflows/
        └── ci.yml
```

---

## What Has Been Implemented

### 1. Local Kubernetes & ArgoCD

* Created a local Kubernetes cluster using Kind
* Installed ArgoCD in the cluster
* Configured an ArgoCD Application pointing to this Git repository
* Enabled auto-sync, self-healing, and namespace auto-creation

---

### 2. Custom Docker Application

* Created a simple application (`index.html`)
* Built a Docker image using a custom Dockerfile
* Pushed the image to Docker Hub
* Replaced the default nginx image with the custom image via GitOps

---

### 3. GitOps Deployment

* Kubernetes manifests (Deployment, Service, Ingress) are stored in Git
* ArgoCD continuously watches the repo
* Any change in manifests is automatically deployed to the cluster
* No manual `kubectl apply` is used for application deployment

---

### 4. CI Pipeline with GitHub Actions

* Added a GitHub Actions workflow that:

  * Triggers on changes to `my-app/`
  * Builds a new Docker image
  * Pushes the image to Docker Hub
  * Updates the image tag in `deployment.yaml`
  * Commits and pushes the change back to the repo
* GitHub Secrets are used for Docker Hub authentication
* Proper permissions are configured to allow CI to push changes

---

### 5. Verification & Testing

* CI success verified in GitHub Actions
* Manifest updates verified in Git commit history
* ArgoCD sync status verified in UI
* Running image verified using `kubectl`
* Application verified using Kubernetes port-forwarding
* Ingress configured (optional) for browser access via hostname

---

## How Changes Are Deployed

1. Developer updates application code
2. GitHub Actions builds and pushes a new Docker image
3. CI updates the Kubernetes manifest with the new image tag
4. ArgoCD detects the Git change
5. Kubernetes automatically deploys the new version

---

## Key Concepts Demonstrated

* Git as the single source of truth
* Separation of CI (build) and CD (deploy)
* Secure credential handling using GitHub Secrets
* Automated deployments without direct cluster access
* Self-healing and drift correction using ArgoCD

---

## Status

✅ CI pipeline working
✅ GitOps deployment working
✅ ArgoCD auto-sync verified
✅ Application changes successfully deployed
