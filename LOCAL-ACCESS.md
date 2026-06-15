# 🚀 Local Application and ArgoCD Access Guide

This guide explains how to access the Flask application UI and ArgoCD UI locally after deployment.

---

# 📌 Start Minikube

Verify Minikube is running:

```bash
minikube status
```

If not running:

```bash
minikube start
```

---

# 📌 Verify Kubernetes Resources

Check application pods:

```bash
kubectl get pods -n flask-app
```

Expected:

```text
flask-app-xxxxx   Running
```

---

# 📌 Verify Kubernetes Service

Run:

```bash
kubectl get svc -n flask-app
```

Example:

```text
NAME            TYPE       PORT(S)
flask-service   NodePort   5000:30081/TCP
```

---

# 📌 Open Flask Application UI

Recommended:

```bash
minikube service flask-service -n flask-app
```

This automatically opens the browser.

---

# 📌 Manual Application Access

Get Minikube IP:

```bash
minikube ip
```

Example:

```text
192.168.49.2
```

Open browser:

```text
http://192.168.49.2:30081
```

---

# 📌 Open ArgoCD UI

Recommended:

```bash
minikube service argocd-server -n argocd
```

This automatically opens the browser.

---

# 📌 Manual ArgoCD Access

Check ArgoCD service:

```bash
kubectl get svc -n argocd
```

Example:

```text
argocd-server NodePort 80:32102/TCP,443:32735/TCP
```

Open browser:

```text
https://192.168.49.2:32735
```

---

# 📌 Get ArgoCD Admin Password

Run:

```bash
kubectl get secret argocd-initial-admin-secret \
-n argocd \
-o jsonpath="{.data.password}" | base64 -d
```

---

# 📌 Login Credentials

Username:

```text
admin
```

Password:

```text
<output-from-password-command>
```

---

# 📌 Verify GitHub Actions Workflow

Push code:

```bash
git add .
git commit -m "test deployment"
git push
```

Go to:

```text
GitHub Repository
→ Actions Tab
```

Verify workflow:

```text
Local Minikube Deployment
```

All steps should become green.

---

# 📌 Verify ArgoCD Deployment

Inside ArgoCD UI verify:

```text
STATUS = Synced
HEALTH = Healthy
```

Application Name:

```text
flask-app
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
Kubernetes Restart
       ↓
ArgoCD Sync
       ↓
Updated Application Available
```
