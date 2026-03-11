# Kubernetes DevOps Project with CI/CD

This DevOps project that demonstrates how to containerize a static web application using Docker, deploy it on Kubernetes using Minikube, and automate Docker image build and push using GitHub Actions CI/CD.

---

## Project Overview

This project was built to practice core DevOps concepts in a simple and practical way.

It includes:

- Docker containerization
- Kubernetes deployment using Minikube
- Namespace-based isolation
- ConfigMap usage
- Service exposure using NodePort
- GitHub Actions CI/CD pipeline
- Docker Hub image push
- Troubleshooting common DevOps issues
- 

---

## Architecture

### High-Level Architecture

``text
Developer
   ↓
GitHub Push
   ↓
GitHub Actions CI/CD
   ↓
Build Docker Image
   ↓
Push Image to Docker Hub
   ↓
Kubernetes Deployment
   ↓
Pods (2 replicas)
   ↓
Kubernetes Service (NodePort)
   ↓
User Browser



##KUBERNETES ARCHITECTURE

User
  ↓
Kubernetes Service (NodePort)
  ↓
Deployment
  ↓
Pods (2 replicas)
  ↓
Container (Nginx serving index.html)


## CI/CD ARCHITECTURE

Developer
  ↓
Push Code to GitHub
  ↓
GitHub Actions Workflow Triggered
  ↓
Docker Image Build
  ↓
Docker Image Push to Docker Hub
  ↓
Kubernetes uses latest image


## Tools Used

Docker

Kubernetes

Minikube

kubectl

Git

GitHub

GitHub Actions

Docker Hub

Nginx


### Implementation

### Step 1 — Create Project Structure

mkdir simple-k8s-devops-project

cd simple-k8s-devops-project

mkdir app k8s screenshots

mkdir -p .github/workflows

touch README.md

touch app/index.html app/Dockerfile

touch k8s/namespace.yaml k8s/configmap.yaml k8s/deployment.yaml k8s/service.yaml

git init

git branch -m main

### Step 2 — Create Application

app/index.html
<!DOCTYPE html>
<html>
<head>
<title>Kubernetes DevOps Project</title>
</head>

<body>
<h1>Kubernetes DevOps Project</h1>
<p>Application running inside Kubernetes.</p>
</body>

</html>


### Step 3 — Create Dockerfile

app/Dockerfile
FROM nginx:alpine
COPY index.html /usr/share/nginx/html/index.html
EXPOSE 80


### Step 4 — Build Docker Image

cd app
docker build -t simple-k8s-app:v1 .
docker images


### Step 5 — Test Docker Container

docker run -d -p 8081:80 --name k8s-test simple-k8s-app:v1

Open browser:

http://localhost:8081

Stop container:

docker stop k8s-test
docker rm k8s-test

### Step 6 — Start Kubernetes Cluster

minikube start --driver=docker
minikube status
kubectl get nodes

### Step 7 — Create Kubernetes Namespace
k8s/namespace.yaml
apiVersion: v1
kind: Namespace
metadata:
  name: devops-project

Apply:

kubectl apply -f k8s/namespace.yaml

### Step 8 — Create ConfigMap

k8s/configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
  namespace: devops-project
data:
  APP_ENV: "development"

Apply:

kubectl apply -f k8s/configmap.yaml

### Step 9 — Create Deployment

k8s/deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: simple-k8s-app
  namespace: devops-project

spec:
  replicas: 2

  selector:
    matchLabels:
      app: simple-k8s-app

  template:
    metadata:
      labels:
        app: simple-k8s-app

   spec:
      containers:
      - name: simple-k8s-app
        image: madhan68/simple-k8s-app:latest
        imagePullPolicy: Always

   ports:
       - containerPort: 80

   envFrom:
       - configMapRef:
            name: app-config

Apply:

kubectl apply -f k8s/deployment.yaml

Check pods:

kubectl get pods -n devops-project

### Step 10 — Create Service

k8s/service.yaml
apiVersion: v1
kind: Service

metadata:
  name: simple-k8s-service
  namespace: devops-project

spec:
  selector:
    app: simple-k8s-app

  ports:
  - port: 80
    targetPort: 80

  type: NodePort

Apply:

kubectl apply -f k8s/service.yaml

### Step 11 — Access the Application

minikube service simple-k8s-service -n devops-project --url

Open the generated URL in the browser.

### Step 12 — Verify Kubernetes Resources

kubectl get all -n devops-project
kubectl describe deployment simple-k8s-app -n devops-project
kubectl logs <pod-name> -n devops-project

### Step 13 — Setup CI/CD Pipeline

Create workflow file:

.github/workflows/docker-build.yml
name: Build and Push Docker Image

on:
  push:
    branches:
      - main

jobs:
  docker:

   runs-on: ubuntu-latest

   steps:
      - uses: actions/checkout@v4

   - name: Login to DockerHub
        uses: docker/login-action@v3
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_PASSWORD }}

      - name: Build Image
        run: docker build -t ${{ secrets.DOCKER_USERNAME }}/simple-k8s-app:latest ./app

      - name: Push Image
        run: docker push ${{ secrets.DOCKER_USERNAME }}/simple-k8s-app:latest
        
### Step 14 — Add GitHub Secrets

In GitHub Repository Settings → Secrets → Actions

Add:

DOCKER_USERNAME = madhan68
DOCKER_PASSWORD = <docker hub access token>

### Step 15 — Push Code

git add .
git commit -m "Added Kubernetes deployment and CI/CD pipeline"
git push origin main

This triggers the GitHub Actions CI/CD pipeline.

### Step 16 — Redeploy Application

After CI/CD pushes the image:

kubectl rollout restart deployment simple-k8s-app -n devops-project
CI/CD Pipeline Flow

Developer changes code
        ↓
Push to GitHub
        ↓
GitHub Actions triggers
        ↓
Docker image build
        ↓
Docker image pushed to Docker Hub
        ↓
Kubernetes deployment uses updated image
        ↓
Application updated






