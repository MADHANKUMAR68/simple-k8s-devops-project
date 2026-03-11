# Simple Kubernetes DevOps Project

## Project Overview
This is a simple DevOps project where a static web application is containerized using Docker and deployed into Kubernetes using Minikube.

The project demonstrates:
- Docker containerization
- Kubernetes Deployment
- Kubernetes Service
- Kubernetes ConfigMap
- Namespace-based resource isolation
- Basic troubleshooting and verification

---

## Tools Used
- Docker
- Kubernetes
- Minikube
- kubectl
- Git
- GitHub
- Nginx

---

## Project Structure

simple-k8s-devops-project/
├── app/
│   ├── index.html
│   └── Dockerfile
├── k8s/
│   ├── namespace.yaml
│   ├── deployment.yaml
│   ├── service.yaml
│   └── configmap.yaml
├── monitoring/
│   ├── prometheus/
│   └── grafana/
├── screenshots/
├── README.md
└── .gitignore

---

## Application Flow
1. Create a simple HTML application
2. Containerize it using Docker
3. Build the Docker image
4. Run and test container locally
5. Start Kubernetes cluster using Minikube
6. Create Namespace
7. Deploy app using Deployment
8. Expose app using Service
9. Store configuration using ConfigMap
10. Verify pods, deployment, and service health

---

## Kubernetes Resources Used

### Namespace
Used to isolate project resources.

### Deployment
Used to manage application pods and replicas.

### Service
Used to expose the application.

### ConfigMap
Used to store application configuration separately from the image.

---

## Commands Used

### Build Docker Image
```bash
docker build -t simple-k8s-app:v1 .
