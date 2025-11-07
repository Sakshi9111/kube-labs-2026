# SynChat Application

A Kubernetes-based chat application deployed on Minikube with Gateway API for ingress routing.

## 📋 Prerequisites

- **Docker** - Container runtime
- **Minikube** - Local Kubernetes cluster
- **kubectl** - Kubernetes command-line tool
- **Git** - Version control

### Installation Links

- [Docker](https://docs.docker.com/get-docker/)
- [Minikube](https://minikube.sigs.k8s.io/docs/start/)
- [kubectl](https://kubernetes.io/docs/tasks/tools/)

## 🚀 Quick Start

### 1. Start Minikube

```bash
# Start Minikube with recommended resources
minikube start --cpus=4 --memory=8192 --driver=docker

# Verify cluster is running
kubectl cluster-info

```

### 2. Enable Required Addons

```bash
# Enable ingress and metrics
minikube addons enable ingress
minikube addons enable metrics-server


```

#### 3. Deploy apps and services

```bash

# Clone the repository
git clone <your-repo-url>
cd kube-labs-2026

# Apply Kubernetes manifests
kubectl apply -f ---

# Verify deployments
kubectl get pods
kubectl get services
kubectl get gateway
kubectl get httproute

```

### 4. Configure DNS
```bash 
# For Linux/Mac
sudo nano /etc/hosts

# For Windows (as Administrator)
notepad C:\Windows\System32\drivers\etc\hosts
 
add these

127.0.0.1   synchat.internal
127.0.0.1   synchatapi.internal



```

### 5. portforward
```bash

kubectl port-forward -n envoy-gateway-system svc/envoy-default-app-gateway-[] 8080:80

```

### 6. Access apps

```bash 

# Access the frontend
curl http://synchat.internal:8080

# Access the API
curl http://synchatapi.internal:8080/api/health

# Or in browser
http://synchat.internal:8080
http://synchatapi.internal:8080

```