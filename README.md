# ArgoCD GitOps Repository

This repository contains the Kubernetes manifests and ArgoCD Application definitions used to manage an Amazon EKS cluster using the GitOps approach.

ArgoCD continuously monitors this repository and synchronizes any changes to the Kubernetes cluster, making Git the **Single Source of Truth**.

---

## Repository Structure

```
.
├── app-of-apps.yaml          # Root Application (App of Apps)
├── roboshop-apps.yaml        # Roboshop applications
├── applications/             # ArgoCD Application manifests
├── namespaces/               # Namespace manifests
├── storage-classes/          # StorageClass definitions
└── roboshop/                 # Roboshop application manifests
```

---

## Architecture

```
                GitHub Repository
                       │
                       │
                 Watches Changes
                       │
                       ▼
                  ArgoCD Server
                       │
                Automatic Sync
                       │
                       ▼
                 Amazon EKS Cluster
                       │
        ┌──────────────┼──────────────┐
        │              │              │
   Namespaces    StorageClasses   Applications
```

---

## GitOps Workflow

```
Developer
     │
     ▼
Feature Branch
     │
     ▼
Pull Request
     │
     ▼
Merge to Main
     │
     ▼
GitHub Repository
     │
     ▼
ArgoCD detects changes
     │
     ▼
Syncs automatically
     │
     ▼
Amazon EKS
```

---

## Repository Components

### app-of-apps.yaml

Implements the **App of Apps** pattern.

Instead of manually creating each ArgoCD Application, this root application manages every application inside the `applications/` directory automatically.

---

### applications/

Contains ArgoCD Application resources such as:

- Namespace Management
- Storage Classes
- AWS Load Balancer Controller
- Roboshop Applications

Each application defines:

- Source Repository
- Target Revision
- Destination Cluster
- Sync Policy

---

### namespaces/

Stores Kubernetes Namespace manifests.

Examples:

- roboshop-dev
- roboshop-uat
- roboshop-prod

Adding or removing namespace YAML files automatically creates or deletes namespaces in the cluster.

---

### storage-classes/

Contains Kubernetes StorageClass resources.

Example:

- Amazon EBS CSI StorageClass

Managed entirely through GitOps.

---

### roboshop/

Contains ArgoCD Application definitions for Roboshop microservices.

Example:

- catalogue-dev
- catalogue-uat
- future production applications

ArgoCD continuously watches these applications and deploys them whenever Helm values change.

---

### roboshop-apps.yaml

Creates the Roboshop parent application.

It monitors the `roboshop/` folder and automatically deploys every application inside it.

---

## Features

- GitOps-based Kubernetes management
- Automatic synchronization
- Self Healing
- Automatic Pruning
- Namespace management
- StorageClass management
- AWS Load Balancer Controller deployment
- Roboshop application deployment
- App of Apps architecture
- Environment-specific deployments

---

## ArgoCD Sync Policy

Every application is configured with:

- Automated Sync
- Self Heal
- Prune

Meaning:

- Any Git change is automatically deployed.
- Manual cluster changes are reverted.
- Deleted manifests are removed from the cluster automatically.

---

## Deployment Flow

```
GitHub
   │
   ▼
ArgoCD
   │
   ▼
App of Apps
   │
   ├──────────► Namespaces
   ├──────────► Storage Classes
   ├──────────► AWS Load Balancer Controller
   └──────────► Roboshop Applications
```

---

## CI/CD Integration

GitHub Actions performs Continuous Integration (CI):

- Build
- Unit Tests
- Security Scans
- Docker Image Build
- Push Image to Amazon ECR
- Update Helm values with new image version

ArgoCD performs Continuous Deployment (CD):

- Detects updated Helm values
- Synchronizes the application
- Deploys the latest image to Amazon EKS

---

## Benefits

- Git is the Single Source of Truth
- No manual kubectl apply
- Automatic reconciliation
- Easy rollback
- Version-controlled infrastructure
- Scalable Kubernetes management
- Declarative deployments

---

## Technologies Used

- ArgoCD
- Kubernetes
- Amazon EKS
- Helm
- GitHub
- GitHub Actions
- AWS Load Balancer Controller
- Amazon EBS CSI Driver

---

## Getting Started

1. Install ArgoCD on the Kubernetes cluster.
2. Apply `app-of-apps.yaml`.
3. ArgoCD automatically creates all child applications.
4. Push changes to GitHub.
5. ArgoCD detects and synchronizes changes automatically.



