# Kubernetes Setup & Commands

### Install Kubernetes CLI
```bash
choco install kubernetes-cli -y
```

### Install Minikube
```bash
choco install minikube -y
```

### Start Kubernetes Cluster (Docker Driver)
```bash
minikube start --driver=docker
```

### Verify Cluster
```bash
kubectl cluster-info
```

### Get Running Nodes
```bash
kubectl get node
```

### Create Deployment
```bash
kubectl create deployment hello-k8s --image=nginx
```

### Expose Deployment Port
```bash
kubectl expose deployment hello-k8s --type=NodePort --port=80
```

### List Deployments
```bash
kubectl get deployments
```

### List Pods
```bash
kubectl get pods
```

### List Services
```bash
kubectl get svc
```

### Watch Pod Status (Live)
```bash
kubectl get pods -w
```

### Check the Service
```bash
minikube service hello-k8s
```

### Check the Service URL
```bash
minikube service hello-k8s --url
```

### Delete Service
```bash
kubectl delete svc hello-k8s
# or
kubectl delete service hello-k8s
```

- Removes the Kubernetes Service named `hello-k8s`
- Stops exposing the application through that Service
- Does **not** delete the Pods or Deployment
- Application continues running inside the cluster

```
Before deleting Service          After deleting Service
────────────────────────         ──────────────────────
Deployment                       Deployment
    ↓                                ↓
   Pods                             Pods
    ↓
 Service                        (No external access)
    ↓
 Users
```

### Delete Deployment
```bash
kubectl delete deployment hello-k8s
```

- Deletes the Deployment object
- Deletes the ReplicaSet created by the Deployment
- Deletes all Pods managed by that Deployment

```
Before                           After
──────────────────────           ─────────────────
Deployment                       (Nothing remains)
   └── ReplicaSet
         ├── Pod-1
         ├── Pod-2
         └── Pod-3
```

---

## Notes: Core Concepts

### 1. Pod = The Running Application

A Pod is the smallest deployable unit in Kubernetes. It contains one or more containers.

```
Pod
 └── nginx container

# or

Pod
 ├── frontend container
 └── logging sidecar container
```

Create a Pod directly:
```bash
kubectl run nginx --image=nginx
```

Check Pods:
```bash
kubectl get pods
```

Example output:
```
NAME                      READY   STATUS
nginx-6f75d4f8d9-x7k2m    1/1     Running
```

> **Problem with Pods:** Pods are temporary. If a Pod crashes, it does not automatically restart unless something manages it.

---

### 2. Deployment = Pod Manager

A Deployment manages Pods and ensures the desired number are always running.

```
Deployment
    └── 3 Pods
```

You tell Kubernetes: *"I want 3 nginx Pods running."*

```yaml
replicas: 3
```

Kubernetes ensures:
```
Pod 1 ✅
Pod 2 ✅
Pod 3 ✅
```

If one dies, the Deployment immediately creates a replacement:
```
Pod 1 ❌  →  New Pod 1 ✅
```

Scale a Deployment:
```bash
kubectl scale deployment nginx --replicas=5
```

**Benefits of Deployments:**
- Self-healing
- Scaling
- Rolling updates
- Rollbacks

---

### 3. Service = Network Access to Pods

Pods get random IP addresses that change when a Pod dies and a new one starts:

```
Pod A = 10.244.0.1
Pod B = 10.244.0.2
Pod C = 10.244.0.3

Pod A dies → 10.244.0.1 ❌
New Pod   → 10.244.0.4 ✅  (IP changed)
```

A **Service** solves this by providing a stable IP and DNS name:

```
hello-k8s.default.svc.cluster.local
```

```
Service
   ↓  load balances
 ┌─┴──────────────┐
Pod A    Pod B    Pod C
```

Traffic is automatically load balanced:
```
Request 1 → Pod A
Request 2 → Pod B
Request 3 → Pod C
```

---

### Real-World Analogy: Restaurant

| Kubernetes   | Restaurant Analogy                          |
|--------------|---------------------------------------------|
| Pods         | Chefs — they do the actual cooking          |
| Deployment   | Manager — ensures 3 chefs are always on shift |
| Service      | Reception desk — forwards orders to chefs   |

---

### Full Flow

```
Deployment
     │
     ▼
   Pods
     │
     ▼
  Service
     │
     ▼
   Users
```

Your commands create this flow:

```bash
kubectl create deployment hello-k8s --image=nginx
# Creates: Deployment → Pod

kubectl expose deployment hello-k8s --type=NodePort --port=80
# Creates: Service → Pod
```

```
Browser → Service → Pod
```

---

### Quick Reference Table

| Resource   | Purpose                                              |
|------------|------------------------------------------------------|
| Pod        | Runs one or more containers                          |
| Deployment | Manages Pods: scaling, updates, self-healing         |
| Service    | Stable networking and load balancing to Pods         |

**Interview answer:**
> "A Pod is where the application container runs. A Deployment manages Pods and ensures the desired number of replicas are available. A Service provides a stable endpoint and load balancing so clients can access Pods even when Pod IPs change."


---

## Point of Confusion: Pod vs Container

> Many people assume **Pod = Container**, but in Kubernetes: **Pod ≠ Container**
>
> A Pod is a **wrapper** around one or more containers.

### Layered View

```
Physical Server
   └── Virtual Machine
         └── Docker Container

Kubernetes Node
   └── Pod
         └── Container(s)
```

---

### Docker World vs Kubernetes World

**Docker** — you run containers directly:
```bash
docker run nginx
```
```
Docker Host
   └── nginx container
```

**Kubernetes** — does not deploy containers directly, it deploys Pods:
```bash
kubectl create deployment nginx --image=nginx
```
```
Deployment
   └── Pod
         └── nginx container
```

Notice the extra layer — the container lives *inside* the Pod.

---

### Why Did Kubernetes Create Pods?

Because sometimes multiple containers need to work together very closely.

```
Pod
 ├── Web Container (Nginx)
 └── Logging Container (Fluentd)
```

Both containers in the same Pod:
- Share the same IP address
- Share `localhost`
- Can share storage volumes
- Start and stop together

Kubernetes treats them as a single deployable unit.

---

### Most Real-World Cases

In 90% of cases, a Pod has just one container:

```
Pod                Pod
 └── nginx    or    └── nodejs-app
```

So people casually say *"My Pod is running"* even though there is a container inside.

---

### Visual Example

```bash
kubectl create deployment hello-k8s --image=nginx
```

Creates:
```
Deployment
    │
    ▼
   Pod
    │
    ▼
nginx container
```

Check the Pod:
```bash
kubectl get pods
# hello-k8s-7fd8b6f4b9-abcde
```

Inspect it:
```bash
kubectl describe pod hello-k8s-7fd8b6f4b9-abcde
```

Output:
```
Containers:
  nginx:
    Image: nginx
```

```
Pod name       = hello-k8s-7fd8b6f4b9-abcde
Container name = nginx
```

---

### Sidecar Pattern (Multiple Containers in One Pod)

A common pattern where two tightly coupled containers share the same Pod:

```
Pod
 ├── Java App         ← writes logs
 └── Fluent Bit       ← reads logs, ships to Elasticsearch
```

Both run together because they are tightly coupled — the log collector needs direct access to the app's filesystem.

---

### Easy Analogy

| Kubernetes  | House Analogy                              |
|-------------|--------------------------------------------|
| Pod         | The house (provides network, storage, env) |
| Container   | A person living in the house               |

```
House (Pod)          House (Pod)
 └── Person           ├── Person A
                      ├── Person B
                      └── Person C
```

---

### Interview Answer

> "A Pod is the smallest deployable unit in Kubernetes. It is not a container itself — it is a logical wrapper that holds one or more containers sharing the same network namespace, IP address, and storage. In most cases a Pod contains a single container, but Kubernetes allows multiple tightly coupled containers to run together in the same Pod."