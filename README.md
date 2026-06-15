# 🚀 Capstone Project

# Local CI/CD GitOps Pipeline using Flask, Docker, GitHub Actions, Kubernetes and ArgoCD

---

# 📌 Project Overview

This capstone project demonstrates a complete modern DevOps and GitOps deployment workflow using:

* Flask Application
* Docker Containerization
* GitHub Actions CI/CD
* Kubernetes Orchestration
* ArgoCD GitOps Continuous Delivery

The project is designed to simulate an enterprise-style software delivery pipeline completely in a local environment without requiring cloud infrastructure.

The application is automatically built, deployed, synchronized and managed using Kubernetes and ArgoCD whenever code changes are pushed to GitHub.

---

# 🎯 Project Objective

The primary objective of this project is to implement an end-to-end CI/CD and GitOps pipeline that automates:

* Application Build
* Containerization
* Deployment
* Synchronization
* Rollout Updates
* Continuous Delivery

This project helps demonstrate practical DevOps concepts including automation, orchestration, containerization and GitOps workflows.

---

# 🏗️ Architecture

```text
Developer Push
       ↓
GitHub Repository
       ↓
GitHub Actions (Self-hosted Runner)
       ↓
Docker Image Build (Local)
       ↓
Kubernetes Rollout Restart
       ↓
ArgoCD Synchronization
       ↓
Application Deployment
```

---

# 🧰 Tech Stack Used

| Technology     | Purpose                    |
| -------------- | -------------------------- |
| Flask          | Python Web Framework       |
| HTML5          | Frontend Structure         |
| CSS3           | Styling and Animations     |
| Docker         | Containerization           |
| GitHub Actions | CI/CD Automation           |
| Kubernetes     | Container Orchestration    |
| ArgoCD         | GitOps Continuous Delivery |
| Gunicorn       | Production WSGI Server     |
| Git            | Version Control            |

---

# 📖 Technologies Explanation

## 🐍 Flask

Flask is a lightweight Python web framework used to build the application backend.

Features:

* Lightweight
* Fast development
* Easy routing
* Easy integration with Docker

---

## 🐳 Docker

Docker is used to package the application into a portable container.

Benefits:

* Environment consistency
* Easy deployment
* Lightweight virtualization
* Fast startup

---

## ⚡ GitHub Actions

GitHub Actions automates the CI/CD workflow.

Responsibilities:

* Detect code changes
* Build Docker image
* Trigger Kubernetes rollout restart

---

## ☸️ Kubernetes

Kubernetes manages container deployment and orchestration.

Features:

* Self-healing
* Auto-restart
* Rolling updates
* Container orchestration

---

## 🚀 ArgoCD

ArgoCD provides GitOps-based continuous delivery for Kubernetes.

Responsibilities:

* Watch Git repository
* Synchronize manifests
* Maintain desired state
* Continuous deployment

---

# 📁 Project Structure

```text
docker-gitactions/
├── app.py
├── requirements.txt
├── Dockerfile
├── .dockerignore
├── README.md
│
├── templates/
│   └── index.html
│
├── static/
│   └── style.css
│
├── .github/
│   └── workflows/
│       └── ci.yml
│
├── k8s/
│   ├── namespace.yaml
│   ├── deployment.yaml
│   └── service.yaml
│
└── argocd/
    └── application.yaml
```

---

# 🌐 Frontend Features

The Flask web application includes:

* Responsive UI
* Navigation Bar
* Smooth Scrolling
* Fade Animations
* Modern DevOps Dashboard
* Technology Explanation Sections

---

# ⚙️ Prerequisites

Install the following:

* Python 3
* Docker
* Kubernetes
* kubectl
* Git
* ArgoCD
* GitHub Self-hosted Runner

---

# 🐳 Docker Build

## Build Docker Image

```bash
docker build -t flask-app:latest .
```

---

# ▶️ Run Container

```bash
docker run -p 5000:5000 flask-app:latest
```

---

# 🌐 Access Application

```text
http://localhost:5000
```

---

# ☸️ Kubernetes Deployment

## Apply Kubernetes Files

```bash
kubectl apply -f k8s/
```

---

# 🚀 ArgoCD Installation and Access Guide (Minikube)

This guide explains how to install ArgoCD on Minikube, expose the ArgoCD UI, retrieve the admin password and access the dashboard from the browser.

---

# 📌 Step 1 — Create ArgoCD Namespace

Create a dedicated namespace for ArgoCD resources.

```bash
kubectl create namespace argocd
```

Expected Output:

```text
namespace/argocd created
```

---

# 📌 Step 2 — Install ArgoCD

Install all ArgoCD components into the Kubernetes cluster.

```bash
kubectl apply -n argocd \
-f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

This installs:

* ArgoCD API Server
* Application Controller
* Repo Server
* Redis
* Dex Authentication Server
* Notifications Controller

---

# 📌 Step 3 — Verify ArgoCD Pods

Check whether all ArgoCD pods are running successfully.

```bash
kubectl get pods -n argocd
```

Expected Output:

```text
NAME                                               READY   STATUS    RESTARTS
argocd-application-controller-0                    1/1     Running   0
argocd-applicationset-controller-xxxxx             1/1     Running   0
argocd-dex-server-xxxxx                            1/1     Running   0
argocd-notifications-controller-xxxxx              1/1     Running   0
argocd-redis-xxxxx                                 1/1     Running   0
argocd-repo-server-xxxxx                           1/1     Running   0
argocd-server-xxxxx                                1/1     Running   0
```

Important:

All pods should show:

```text
STATUS = Running
```

---

# 📌 Step 4 — Expose ArgoCD UI

By default, ArgoCD server uses ClusterIP service which is accessible only inside the cluster.

Convert it into a NodePort service:

```bash
kubectl patch svc argocd-server \
-n argocd \
-p '{"spec":{"type":"NodePort"}}'
```

Expected Output:

```text
service/argocd-server patched
```

---

# 📌 Step 5 — Check ArgoCD Service Port

View the NodePort assigned to the ArgoCD server.

```bash
kubectl get svc argocd-server -n argocd
```

Example Output:

```text
NAME            TYPE       CLUSTER-IP      EXTERNAL-IP   PORT(S)
argocd-server   NodePort   10.x.x.x        <none>        80:32102/TCP,443:32735/TCP
```

Important Ports:

* HTTP Port → 32102
* HTTPS Port → 32735

---

# 📌 Step 6 — Get Minikube IP

Retrieve the Minikube cluster IP address.

```bash
minikube ip
```

Example Output:

```text
192.168.49.2
```

---

# 📌 Step 7 — Access ArgoCD UI

Use browser and open:

HTTP:

```text
http://192.168.49.2:32102
```

OR HTTPS:

```text
https://192.168.49.2:32735
```

Recommended:
Use HTTPS.

---

# 📌 Step 8 — Browser Security Warning

Since ArgoCD uses a self-signed certificate locally, the browser may show:

```text
Your connection is not private
```

This is normal for local environments.

Click:

```text
Advanced
→ Proceed
```

---

# 📌 Step 9 — Get ArgoCD Admin Password

Retrieve the default ArgoCD admin password.

```bash
kubectl get secret argocd-initial-admin-secret \
-n argocd \
-o jsonpath="{.data.password}" | base64 -d
```

Example Output:

```text
fKx9AbCdEf123
```

---

# 📌 Step 10 — Login to ArgoCD

Username:

```text
admin
```

Password:

```text
<output-from-previous-command>
```

---

# 📌 Step 11 — Recommended Minikube Method

If browser cannot open Minikube IP directly, use:

```bash
minikube service argocd-server -n argocd
```

This automatically:

* Creates proper tunnel
* Generates accessible URL
* Opens browser automatically

Example Output:

```text
|-----------|----------------|-------------|------------------------|
| NAMESPACE |      NAME      | TARGET PORT |          URL           |
|-----------|----------------|-------------|------------------------|
| argocd    | argocd-server  | https/443   | http://127.0.0.1:54023 |
|-----------|----------------|-------------|------------------------|
```

Open the generated URL in browser.

---

# 📌 Step 12 — Verify ArgoCD Dashboard

After successful login, you should see:

* ArgoCD Dashboard
* Cluster Information
* Applications
* Sync Status
* Deployment Health
* Repository Information

---

# 📌 Step 13 — Deploy Application into ArgoCD

Apply the ArgoCD application manifest:

```bash
kubectl apply -f argocd/application.yaml
```

Expected Output:

```text
application.argoproj.io/flask-app created
```

---

# 📌 Step 14 — Verify ArgoCD Application

Check deployed applications:

```bash
kubectl get applications -n argocd
```

Expected Output:

```text
NAME        SYNC STATUS   HEALTH STATUS
flask-app   Synced        Healthy
```

---

# 📌 Step 15 — Access Flask Application

Check Flask service:

```bash
kubectl get svc -n flask-app
```

Example:

```text
NAME            TYPE       CLUSTER-IP      EXTERNAL-IP   PORT(S)
flask-service   NodePort   10.x.x.x        <none>        5000:30081/TCP
```

Open:

```text
http://192.168.49.2:30081
```

OR:

```bash
minikube service flask-service -n flask-app
```

---

# 📌 Complete Deployment Flow

```text
Developer Push
       ↓
GitHub Actions
       ↓
Docker Build
       ↓
Minikube Image Load
       ↓
Kubernetes Deployment
       ↓
ArgoCD Synchronization
       ↓
Application Available
```

---

# 📌 Technologies Used

* Flask
* Docker
* GitHub Actions
* Kubernetes
* Minikube
* ArgoCD
* Gunicorn
* GitOps

---

# 📌 Learning Outcomes

This implementation demonstrates:

✅ CI/CD Automation
✅ GitOps Deployment
✅ Kubernetes Orchestration
✅ Containerization
✅ Local DevOps Environment
✅ Continuous Synchronization
✅ Infrastructure as Code
✅ Self-hosted Runner Integration

---

# 🔄 CI/CD Workflow

Whenever code is pushed to GitHub:

1. GitHub Actions triggers
2. Docker image builds locally
3. Kubernetes deployment restarts
4. ArgoCD synchronizes manifests
5. Updated application becomes available

---

# 📦 Kubernetes Resources Used

This project uses:

* Namespace
* Deployment
* Service
* Probes
* ArgoCD Application

---

# 🔐 Local Deployment Strategy

This project uses:

```yaml
imagePullPolicy: Never
```

Reason:

* Images are built locally
* No DockerHub required
* Fully offline/local deployment supported

---

# 🧪 Verification Commands

## Check Pods

```bash
kubectl get pods -n flask-app
```

---

## Check Services

```bash
kubectl get svc -n flask-app
```

---

## Check ArgoCD Pods

```bash
kubectl get pods -n argocd
```

---

# 📊 Project Summary

This capstone project successfully demonstrates:

✅ CI/CD Automation
✅ Docker Containerization
✅ Kubernetes Deployment
✅ GitOps Continuous Delivery
✅ Local Self-hosted Runner Deployment
✅ ArgoCD Synchronization
✅ Automated Rollout Updates
✅ DevOps Workflow Automation

The project provides hands-on experience with modern DevOps practices and enterprise deployment workflows.

---

# 📚 Learning Outcomes

After completing this project, the following concepts are understood:

* CI/CD Pipelines
* GitOps
* Containerization
* Kubernetes Deployment
* Infrastructure Automation
* Continuous Delivery
* Self-hosted GitHub Runners
* DevOps Lifecycle

---

# 👨‍💻 Author

Sanjeev Tripurari

---

# 🚀 Future Improvements

Possible future enhancements:

* Helm Integration
* Ingress Controller
* HTTPS/TLS
* Monitoring with Prometheus & Grafana
* Multi-environment Deployment
* ArgoCD Image Updater
* Horizontal Pod Autoscaling
* Production-grade Security
