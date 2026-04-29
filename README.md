# 🚀 Docker + GitHub Actions (Local Deployment)

## 📌 Overview

This project demonstrates how to:

* Build a Dockerized Flask application
* Use GitHub Actions for CI/CD
* Deploy the application **directly to your local machine** using a **self-hosted runner**

Unlike cloud deployments (e.g., EC2), this setup runs everything on your own system.

---

## 🏗️ Architecture

```
Developer → Push Code → GitHub → Self-Hosted Runner (Local Machine)
                                         ↓
                                   Docker Build
                                         ↓
                                   Container Run
```

---

## 📁 Project Structure

```
.
├── app.py
├── requirements.txt
├── Dockerfile
└── .github/
    └── workflows/
        └── deploy.yml
```

---

## ⚙️ Application Details

### 🔹 Flask App (`app.py`)

* Simple web server
* Runs on port `5000`
* Accessible via browser

### 🔹 Dockerfile

* Uses Python 3.9 slim image
* Installs dependencies
* Runs Flask app

---

## 🐳 Prerequisites

Make sure the following are installed on your local machine:

* Docker (e.g., Docker Desktop)
* Git
* GitHub account

---

## 🧪 Run Locally (Without GitHub Actions)

### 1. Build Docker Image

```
docker build -t my-local-app .
```

### 2. Run Container

```
docker run -p 5000:5000 my-local-app
```

### 3. Access Application

```
http://localhost:5000
```

---

## ⚡ GitHub Actions Setup

### Step 1: Add Workflow File

Create:

```
.github/workflows/deploy.yml
```

### Step 2: Workflow Configuration

This workflow:

* Triggers on push to `main`
* Runs on self-hosted runner
* Builds Docker image
* Stops old container
* Starts new container

---

## 🖥️ Self-Hosted Runner Setup

### 1. Go to Repository Settings

* Navigate to:

```
Settings → Actions → Runners → New self-hosted runner
```

### 2. Run Commands on Local Machine

Download runner:

```
mkdir actions-runner && cd actions-runner
curl -o runner.tar.gz -L https://github.com/actions/runner/releases/latest/download/actions-runner-linux-x64.tar.gz
tar xzf runner.tar.gz
```

Configure runner:

```
./config.sh --url https://github.com/<your-username>/<repo-name> --token <TOKEN>
```

Start runner:

```
./run.sh
```

> ⚠️ Keep this terminal running

---

## 🚀 Deployment Flow

1. Push code to GitHub:

```
git add .
git commit -m "update"
git push
```

2. GitHub Actions triggers workflow

3. Workflow runs on your local machine

4. Docker container is rebuilt and restarted

---

## 🌐 Access Application

After deployment:

```
http://localhost:5000
```

---

## ❗ Troubleshooting

### 🔴 Container exits immediately

* Ensure Flask runs with:

```
app.run(host="0.0.0.0", port=5000)
```

---

### 🔴 Workflow not triggering

* Ensure correct path:

```
.github/workflows/deploy.yml
```

---

### 🔴 Runner not picking jobs

* Make sure runner is running:

```
./run.sh
```

---

### 🔴 Port not accessible

* Check if container is running:

```
docker ps
```

---

## 🔐 Notes

* No cloud infrastructure required
* No SSH or public IP needed
* All deployments happen locally

---

