# Learn Kubernetes with Irfan

A hands-on, beginner-friendly guide to learning Kubernetes from zero — written in simple English with real-world examples and visualizations.

---

## What is This?

This repository is a structured learning series for Kubernetes. Each document builds on the previous one, starting from what Kubernetes is all the way to running real workloads. Every topic includes Mermaid diagrams, command explanations, and practical YAML examples you can run yourself.

---

## Repository Structure

```
kubernetes/
├── kubernetes Guide/        ← Step-by-step learning series
│   ├── 01. Overview.md
│   ├── 02. how to install kubernetes.md
│   ├── 03. Core Concept k8s.md
│   └── 04. Deployments_Pods_ReplicaSets.md
│
├── kubernetes Projects/     ← Hands-on YAML examples to practice
│   └── 04. Deployments_Pods_ReplicaSets.yaml
│
└── README.md
```

---

## Learning Path

| Step | Topic | Guide |
|------|-------|-------|
| 01 | What is Kubernetes & Architecture | [01. Overview.md](kubernetes%20Guide/01.%20Overview.md) |
| 02 | Install kubectl & Minikube on Windows | [02. how to install kubernetes.md](kubernetes%20Guide/02.%20how%20to%20install%20kubernetes.md) |
| 03 | Core Concepts — Pods, Deployments, Services | [03. Core Concept k8s.md](kubernetes%20Guide/03.%20Core%20Concept%20k8s.md) |
| 04 | Deployments, ReplicaSets & Pods in Depth | [04. Deployments_Pods_ReplicaSets.md](kubernetes%20Guide/04.%20Deployments_Pods_ReplicaSets.md) |

---

## Hands-on Practice

| Project | What it covers | File |
|---------|---------------|------|
| 04 | Bare Pods, Deployments, Labels, Scaling | [04. Deployments_Pods_ReplicaSets.yaml](kubernetes%20Projects/04.%20Deployments_Pods_ReplicaSets.yaml) |

---

## What You Will Learn

- Kubernetes architecture — Control Plane, Worker Nodes, etcd, Scheduler
- Core objects — Pods, ReplicaSets, Deployments, Services
- How to run a local cluster on Windows using Minikube
- Writing and applying YAML manifests
- `kubectl` commands explained with examples
- Labels, selectors, and how objects connect to each other
- Real-world analogies and visual diagrams for every concept

---

## Tools Used

| Tool | Purpose |
|------|---------|
| `kubectl` | Kubernetes CLI — sends commands to the cluster |
| Minikube | Runs a single-node Kubernetes cluster on your laptop |
| Docker Desktop | Container runtime required by Minikube |
| Chocolatey | Windows package manager used to install the tools |

---

## Quick Start

```bash
# 1. Install tools (Windows — run PowerShell as Administrator)
choco install kubernetes-cli minikube -y

# 2. Start the local cluster
minikube start --driver=docker

# 3. Verify the cluster is running
kubectl get nodes
# NAME       STATUS   ROLES           AGE   VERSION
# minikube   Ready    control-plane   1m    v1.xx.x
```

→ Start with [01. Overview.md](kubernetes%20Guide/01.%20Overview.md) for the full picture.

---

## Resources

| Resource | Link |
|----------|------|
| Official Kubernetes Docs | https://kubernetes.io/docs/ |
| Minikube Docs | https://minikube.sigs.k8s.io/docs/ |
| Chocolatey Install Guide | https://docs.chocolatey.org/en-us/choco/setup/ |
| Learn with Irfan | https://learnwithirfan.com |
