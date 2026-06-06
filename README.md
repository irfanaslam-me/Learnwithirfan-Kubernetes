# Learn Kubernetes with Irfan

A hands-on, structured guide to learning Kubernetes from zero.

## Learning Path

| Step | Topic | File |
|------|-------|------|
| 1 | What is Kubernetes & Architecture | [01. Overview.md](01.%20Overview.md) |
| 2 | Install kubectl & Minikube (Windows) | [02. how to install kubernetes.md](02.%20how%20to%20install%20kubernetes.md) |
| 3 | Core Concepts & Commands Reference | [kube.md](kube.md) |

## What You Will Learn

- Kubernetes architecture (Control Plane, Worker Nodes)
- Core objects: Pods, Deployments, Services
- How to run a local cluster with Minikube
- `kubectl` commands for day-to-day use
- Real-world analogies and interview answers

## Tools Used

| Tool | Purpose |
|------|---------|
| `kubectl` | Kubernetes CLI — talks to the cluster |
| Minikube | Runs a single-node K8s cluster locally |
| Docker Desktop | Container runtime (required by Minikube) |
| Chocolatey | Windows package manager for installation |

## Quick Start

```bash
# 1. Install tools
choco install kubernetes-cli minikube -y

# 2. Start the cluster
minikube start --driver=docker

# 3. Verify
kubectl get nodes
```

→ Start with [01. Overview.md](01.%20Overview.md) for the full picture.
