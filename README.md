# 🚀 EKS Blue-Green Deployment Project

This project demonstrates Blue-Green Deployment using Kubernetes on AWS EKS.

## 📌 Features
- Blue Deployment (v1)
- Green Deployment (v2)
- Kubernetes Services (LoadBalancer)
- ConfigMaps for static content
- AWS EKS Cluster

## 🛠️ Tech Stack
- AWS EKS
- Kubernetes
- Docker
- YAML

## ⚙️ Steps to Run

### 1. Apply ConfigMaps
kubectl create configmap blue-html --from-file=index.html=blue-index.html
kubectl create configmap green-html --from-file=index.html=green-index.html

### 2. Deploy Applications
kubectl apply -f blue-deployment.yaml
kubectl apply -f green-canary.yaml

### 3. Expose Service
kubectl apply -f service.yaml

### 4. Get URL
kubectl get svc

## 🌐 Output
Access the app using LoadBalancer URL.

## 👨‍💻 Author
Shrikant Ghayal
