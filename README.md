# 🚀 LEMICI DEVOPS INTERNSHIP ASSIGNMENT

---

# 1️⃣ PART 1: VERSION CONTROL (GIT)

## 1.0 Create Public Repository & Setup SSH

1. Create a **Public Repository** on GitHub.
2. Initialize Git locally → `git init` → `git add .` → `git commit -m "Initial commit"`
3. Generate SSH key → `ssh-keygen -t ed25519 -C "your-email@example.com"`
4. Add SSH key to GitHub and connect repo → `git remote add origin git@github.com:username/repo.git` → `git push -u origin main`

---

## 1.1 Difference Between `git fetch` and `git pull`

| Feature                   | git fetch  | git pull              |
| ------------------------- | ---------- | --------------------- |
| Downloads Changes         | ✅ Yes      | ✅ Yes                 |
| Merges Automatically      | ❌ No       | ✅ Yes                 |
| Updates Working Directory | ❌ No       | ✅ Yes                 |
| Safe Operation            | ✅ Safe     | ⚠ May Cause Conflicts |
| Combination               | Only fetch | fetch + merge         |

### 1.2 `git fetch`

* Downloads latest changes from remote repository
* Updates remote tracking branch (origin/main)
* Does NOT change current branch

```bash
git fetch origin
```

### 1.3 `git pull`

* Downloads latest changes
* Automatically merges into current branch
* May cause conflicts

```bash
git pull origin main
```

### 1.4 How to Resolve Merge Conflict

1. Open the file
2. Remove conflict markers
3. Keep correct code
4. Save file

```bash
git add filename
git commit -m "Conflict resolved"
git push origin main
```

---

# 2️⃣ PART 2: DOCKER & CONTAINERIZATION

## 2.1 Simple Flask Application

### app.py

```python
from flask import Flask
app = Flask(__name__)

@app.route("/")
def home():
    return "Hello, DevOps Internship Assignment!"

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)
```

### requirements.txt

```
flask
```

---

## 2.2 Dockerfile

```dockerfile
FROM python:3.9-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY app.py .
EXPOSE 5000
CMD ["python", "app.py"]
```

---

## 2.3 Dockerfile vs Image vs Container

| Dockerfile        | Docker Image       | Docker Container |
| ----------------- | ------------------ | ---------------- |
| Blueprint         | Built Package      | Running Instance |
| Text Instructions | App + Dependencies | Executing App    |

---

## 2.4 Running Application Locally

```bash
docker build -t my-flask-app .
docker run -p 5000:5000 my-flask-app
```

Access: [http://localhost:5000](http://localhost:5000)

---

3️⃣ PART 3: KUBERNETES (EKS BASICS)
3.1 Pod vs Deployment vs Service

Pod

Smallest deployable unit in Kubernetes

Runs one or more containers

Shares network and storage

Deployment

Manages and maintains Pods

Ensures the desired number of replicas are running

Supports updates and scaling

Service

Exposes Pods to internal or external traffic

Provides stable networking for Pods

Load balances traffic between Pods

3.2 Why do we need EKS (Managed Kubernetes) instead of running Kubernetes on VMs?

Running Kubernetes directly on Virtual Machines requires manual setup and maintenance.
Managed Kubernetes services like Amazon EKS simplify this process.

Reasons to use EKS:

No Control Plane Management – AWS manages the Kubernetes master nodes.

Automatic Updates & Patching – EKS handles updates and security patches.

High Availability – Control plane is automatically distributed across multiple AWS zones.

Easy Integration with AWS Services – Works with IAM, CloudWatch, Load Balancers, etc.

Reduced Operational Overhead – Developers focus on applications instead of infrastructure.

3.3 Deployment YAML
apiVersion: apps/v1
kind: Deployment
metadata:
  name: flask-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: flask-app
  template:
    metadata:
      labels:
        app: flask-app
    spec:
      containers:
      - name: flask-container
        image: my-flask-app:latest
        ports:
        - containerPort: 5000
3.4 Service YAML
apiVersion: v1
kind: Service
metadata:
  name: flask-service
spec:
  type: LoadBalancer
  selector:
    app: flask-app
  ports:
  - port: 80
    targetPort: 5000

---

# 4️⃣ PART 4: CI/CD PIPELINE

## 4.1 GitHub Actions Workflow

```yaml
name: Docker CI Pipeline

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - name: Checkout Code
      uses: actions/checkout@v3

    - name: Build Docker Image
      run: docker build -t my-flask-app .
```

---

# 5️⃣ PART 5: MONITORING & LOGS

## 5.1 Metrics vs Logs vs Traces

1. Metrics → CPU, Memory, Requests
2. Logs → Errors and events
3. Traces → Request flow across services

---

## 5.2 Debugging Crashed Pod

```bash
kubectl get pods
kubectl describe pod <pod-name>
kubectl logs <pod-name>
kubectl logs <pod-name> --previous
kubectl top pod <pod-name>
```

---

# 6️⃣ PART 6: PROBLEM-SOLVING SCENARIO

## 6.1 Requirements

1. Code on GitHub
2. Containerize application
3. Auto-deploy on merge to main
4. Logs visible to dev team

---

## 6.2 Solution Steps

### Step 1: Containerize

* Create Dockerfile
* Build image
* Push to DockerHub

### Step 2: Deploy to EKS

```bash
kubectl apply -f deployment.yaml
```

### Step 3: Setup CI/CD

* Trigger on push to main
* Build image
* Deploy to EKS

### Step 4: Logging

* Use CloudWatch or EFK stack



