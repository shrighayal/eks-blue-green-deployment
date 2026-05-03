# 🚀 Kubernetes Blue-Green & Canary Deployment on AWS EKS

## 📌 Project Overview

This project demonstrates a **production-style deployment strategy in Kubernetes** using **Blue-Green and Canary deployments** on **AWS EKS**.

The goal is to release new application versions **safely with zero downtime** while ensuring a smooth user experience.

By running **multiple versions simultaneously**, traffic can be gradually shifted from the stable version to the new version.

---

## 🎯 Key Objectives

- Zero downtime deployment  
- Gradual rollout (Canary)  
- Safe production testing  
- Easy rollback  
- Visual verification  

---

## 🏗 Deployment Strategy

### 🔵 Blue Deployment (Stable)
Current stable version serving users.

### 🟢 Green Deployment (New)
New version introduced gradually.

### 🐤 Canary Release
Small portion of traffic goes to new version → then increased gradually.

Traffic control is done using **pod scaling**.

---

## 🏛 Architecture

User → AWS LoadBalancer → Kubernetes Service → Pods (Blue + Green)

---

## 🧰 Tech Stack

- AWS EC2 (Ubuntu)
- AWS EKS
- Kubernetes (kubectl)
- eksctl
- NGINX
- ConfigMaps
- LoadBalancer Service

---

## 📂 Project Structure

eks-blue-green-deployment/
│
├── blue-deployment.yaml
├── green-canary.yaml
├── service.yaml
├── blue-index.html
├── green-index.html
└── README.md

---

## ⚙️ Setup Steps

### 1️⃣ Update System
sudo apt update && sudo apt upgrade -y

---

### 2️⃣ Install Tools

sudo apt install unzip curl -y

# AWS CLI
curl https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip -o awscliv2.zip
unzip -o awscliv2.zip
sudo ./aws/install --update

# kubectl
curl -LO https://dl.k8s.io/release/v1.29.0/bin/linux/amd64/kubectl
chmod +x kubectl
sudo mv kubectl /usr/local/bin/

# eksctl
curl -sLO https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_Linux_amd64.tar.gz
tar -xzf eksctl_Linux_amd64.tar.gz
sudo mv eksctl /usr/local/bin/

---

### 3️⃣ Create EKS Cluster

eksctl create cluster \
--name blue-green-cluster \
--region ap-south-1 \
--nodegroup-name worker-nodes \
--node-type t3.medium \
--nodes 2

Verify:
kubectl get nodes

---

### 4️⃣ Create ConfigMaps

kubectl create configmap blue-html --from-file=index.html=blue-index.html
kubectl create configmap green-html --from-file=index.html=green-index.html

---

### 5️⃣ Deploy Applications

kubectl apply -f blue-deployment.yaml
kubectl apply -f green-canary.yaml

---

### 6️⃣ Expose Service

kubectl apply -f service.yaml

Check:
kubectl get svc

---

### 7️⃣ Access Application

kubectl get svc myapp-service

Open in browser:
http://<EXTERNAL-IP>

Refresh multiple times to see Blue / Green versions

---

## 🔄 Canary Traffic Control

Initial:
Blue → 4 Pods  
Green → 1 Pod  

### 50% Traffic
kubectl scale deployment green-canary --replicas=3  
kubectl scale deployment blue-app --replicas=3  

### Full Promotion
kubectl scale deployment green-canary --replicas=6  
kubectl scale deployment blue-app --replicas=0  

---

## 🔙 Rollback

kubectl scale deployment green-canary --replicas=0  
kubectl scale deployment blue-app --replicas=4  

---

## 🧹 Cleanup

eksctl delete cluster --name blue-green-cluster --region ap-south-1

---

## 📈 DevOps Value

- Zero downtime deployment  
- Safe release strategy  
- Kubernetes traffic control  
- Production-ready architecture  

---

## 📌 Conclusion

This project demonstrates how **Blue-Green and Canary deployments** can be implemented using **Kubernetes on AWS EKS**.

It is a **real-world DevOps project** showcasing safe deployment strategies and cloud-native practices.

---

## 👨‍💻 Author

Shrikant Ghayal
