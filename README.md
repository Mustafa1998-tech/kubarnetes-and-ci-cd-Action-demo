# Kubernetes CKA Demo

This is a simple demo application for Kubernetes CKA certification practice. It demonstrates basic Kubernetes concepts including Deployments, Services, Scaling, and Rollouts.

## Prerequisites

- Minikube
- kubectl
- Docker

## Setup Instructions

### 1. Start Minikube
```bash
minikube start
```

### 2. Build and Load Docker Image
```bash
# Build the Docker image
docker build -t cka-demo:1.0 .

# Load the image into Minikube
minikube image load cka-demo:1.0
```

### 3. Deploy the Application
```bash
# Create deployment
kubectl create deployment cka-demo --image=cka-demo:1.0

# Expose the deployment
kubectl expose deployment cka-demo --type=NodePort --port=80

# Get the service URL
minikube service cka-demo --url
```

### 4. Scale the Application
```bash
# Scale to 3 replicas
kubectl scale deployment cka-demo --replicas=3

# Verify pods
kubectl get pods
```

### 5. Autoscaling
```bash
# Create Horizontal Pod Autoscaler
kubectl autoscale deployment cka-demo --cpu-percent=50 --min=3 --max=5

# Check HPA status
kubectl get hpa
```

### 6. Update and Rollback
```bash
# Update the image
kubectl set image deployment/cka-demo cka-demo=cka-demo:1.1

# Check rollout status
kubectl rollout status deployment/cka-demo

# Rollback if needed
kubectl rollout undo deployment/cka-demo
```

### 7. Cleanup
```bash
kubectl delete deployment cka-demo
kubectl delete svc cka-demo
kubectl delete hpa cka-demo
```

## Verification

1. After deployment, you should see "Hello CKA Demo" when accessing the service URL
2. Use `kubectl get all` to see all resources
3. Check logs with `kubectl logs -l app=cka-demo`
# Trigger CI/CD
