SynChat Application
A Kubernetes-based chat application deployed on Minikube with Gateway API for ingress routing.
📋 Prerequisites

Docker - Container runtime
Minikube - Local Kubernetes cluster
kubectl - Kubernetes command-line tool
Git - Version control

Installation Links

Docker
Minikube
kubectl

🚀 Quick Start
1. Start Minikube
bash# Start Minikube with recommended resources
minikube start --cpus=4 --memory=8192 --driver=docker

# Verify cluster is running
kubectl cluster-info
2. Enable Required Addons
bash# Enable ingress and metrics
minikube addons enable ingress
minikube addons enable metrics-server
3. Install Gateway API CRDs
bash# Install Gateway API resources
kubectl apply -f https://github.com/kubernetes-sigs/gateway-api/releases/download/v1.0.0/standard-install.yaml

# Verify installation
kubectl get crd | grep gateway
4. Deploy the Application
bash# Clone the repository
git clone <your-repo-url>
cd purecode-infra-automation

# Apply Kubernetes manifests
kubectl apply -f k8s/

# Verify deployments
kubectl get pods
kubectl get services
kubectl get gateway
kubectl get httproute
5. Configure Local DNS
Add the following entries to your /etc/hosts file:
bash# For Linux/Mac
sudo nano /etc/hosts

# For Windows (as Administrator)
notepad C:\Windows\System32\drivers\etc\hosts
Add these lines:
127.0.0.1   synchat.internal
127.0.0.1   synchatapi.internal
6. Port Forward Gateway
bash# Forward the gateway service to local port 8080
kubectl port-forward -n default svc/gateway-service 8080:80

# Or if using a different namespace
kubectl port-forward -n <namespace> svc/gateway-service 8080:80
7. Access the Application
Open your browser or use curl:
bash# Access the frontend
curl http://synchat.internal:8080

# Access the API
curl http://synchatapi.internal:8080/api/health

# Or in browser
http://synchat.internal:8080
http://synchatapi.internal:8080
📁 Project Structure
purecode-infra-automation/
├── k8s/
│   ├── deployment.yaml        # Application deployments
│   ├── service.yaml           # Kubernetes services
│   ├── gateway.yaml           # Gateway API Gateway
│   └── httproute.yaml         # HTTP routing rules
├── src/                       # Application source code
├── Dockerfile                 # Container image definition
└── README.md                  # This file
🔧 Configuration
Gateway Configuration
The application uses Kubernetes Gateway API for routing:

Frontend: synchat.internal → SynChat frontend service
API: synchatapi.internal → SynChat API service

Environment Variables
Configure the following in your deployment manifests:
yamlenv:
  - name: API_URL
    value: "http://synchatapi.internal:8080"
  - name: DATABASE_URL
    value: "postgresql://user:pass@db:5432/synchat"
🐛 Troubleshooting
Gateway Not Working
bash# Check gateway status
kubectl get gateway
kubectl describe gateway synchat-gateway

# Check gateway pods
kubectl get pods -A | grep gateway

# View gateway logs
kubectl logs -n <gateway-namespace> <gateway-pod-name>
Application Pods Not Running
bash# Check pod status
kubectl get pods

# Describe pod for issues
kubectl describe pod <pod-name>

# View pod logs
kubectl logs <pod-name>
DNS Resolution Issues
bash# Test DNS inside cluster
kubectl run -it --rm debug --image=busybox --restart=Never -- nslookup synchat.internal

# Verify /etc/hosts entries
cat /etc/hosts | grep synchat
Port Forward Not Working
bash# Kill existing port-forward processes
pkill -f "port-forward"

# Restart with verbose logging
kubectl port-forward -v=9 svc/gateway-service 8080:80
🔄 Development Workflow
Rebuild and Deploy
bash# Build Docker image with Minikube's Docker daemon
eval $(minikube docker-env)
docker build -t synchat:latest .

# Apply changes
kubectl apply -f k8s/

# Restart deployment
kubectl rollout restart deployment synchat-frontend
kubectl rollout restart deployment synchat-api
View Logs
bash# Stream logs from frontend
kubectl logs -f deployment/synchat-frontend

# Stream logs from API
kubectl logs -f deployment/synchat-api

# View all logs from a pod
kubectl logs <pod-name> --all-containers
🧹 Cleanup
bash# Delete all resources
kubectl delete -f k8s/

# Stop Minikube
minikube stop

# Delete Minikube cluster
minikube delete
📊 Monitoring
Check Resource Usage
bash# View resource usage
kubectl top nodes
kubectl top pods

# Access Minikube dashboard
minikube dashboard
