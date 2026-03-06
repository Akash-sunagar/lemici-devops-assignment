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

# 3️⃣ PART 3: KUBERNETES (EKS BASICS)

3.1 Pod vs Deployment vs Service
Pod

Smallest deployable unit in Kubernetes

Runs one or more containers

Shares network and storage

Deployment

Manages Pods

Maintains desired number of replicas

Supports scaling and rolling updates

Service

Exposes Pods to internal or external traffic

Provides stable networking

Load balances traffic between Pods

3.2 Why Do We Need EKS Instead of Running Kubernetes on VMs?

Running Kubernetes manually on Virtual Machines requires manual configuration, management, and maintenance of control plane components.

Managed Kubernetes services like Amazon EKS (Elastic Kubernetes Service) simplify Kubernetes operations.

Advantages of Amazon EKS

No control plane management

Automatic updates and security patches

High availability across multiple availability zones

Easy integration with AWS services (IAM, CloudWatch, Load Balancers)

Reduced operational overhead

---

## 3.2 Deployment YAML
```yaml
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
```

---

## 3.3 Service YAML

```yaml
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
```

---

# 4️⃣ PART 4: CI/CD PIPELINE

## 4.1 GitHub Actions Workflow
4.1 GitHub Actions Workflow

This workflow demonstrates a simple CI/CD pipeline using GitHub Actions.
It performs three basic steps for this assignment:

Builds the Docker image

Runs a simple test step

Simulates pushing the Docker image to DockerHub
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
Checkout Repository – Downloads the project code from GitHub.

Build Docker Image – Builds the Docker container image for the application.

Run Simple Tests – Runs a basic test step (simulated for this assignment).

Simulate Docker Push – Demonstrates pushing the image to DockerHub (simulated without credentials).

---

# 5️⃣ PART 5: MONITORING & LOGS

## 5.1 Metrics vs Logs vs Traces

Explain the difference between metrics, logs, and traces.

Monitoring systems collect different types of data to understand application behavior and system health.

1)Metrics

Metrics are numerical measurements collected over time.

Examples:

a)CPU usage

b)Memory usage

Metrics help in detecting performance issues and system health trends.

2)Logs

Logs are records of events generated by applications or systems.

Examples:

a)Application errors

b)System events

Logs help developers understand what happened in the system at a specific time.

3)Traces

Traces track the journey of a request across multiple services in a distributed system.

Examples:

a)API request flow between microservices

b)Latency between services

Traces help in understanding performance problems across multiple services.

5.2 Question: If your Kubernetes pod crashes, how would you debug the issue? (write the commands and reasoning)

If a Kubernetes pod crashes, the debugging process involves checking the pod status, logs, and configuration.

Step 1: Check Pod Status

1)kubectl get pods

This command shows the current state of all pods and helps identify crashed or restarting pods.

Step 2: Describe the Pod

2)kubectl describe pod <pod-name>

This command provides detailed information about the pod, including events, container errors, and resource issues.

Step 3: Check Pod Logs

3)kubectl logs <pod-name>

Logs show application output and help identify runtime errors.

Step 4: Check Previous Container Logs

4)kubectl logs <pod-name> --previous

If the container restarted, this command retrieves logs from the previous instance.

Step 5: Check Resource Usage

5)kubectl top pod <pod-name>

This command helps determine if the pod crashed due to CPU or memory limits.

What tools would you suggest for monitoring in AWS EKS and why?

1)Prometheus

Open-source monitoring tool

Collects time-series metrics from Kubernetes

Widely used for Kubernetes monitoring

2)Grafana

Visualization tool for monitoring data

Works with Prometheus and CloudWatch

Provides dashboards for system metrics
```
---

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

- Create Dockerfile  
- Build Docker image  
- Push image to DockerHub  

### Step 2: Deploy to EKS

```bash
kubectl apply -f deployment.yaml
```

This command deploys the application to the Kubernetes cluster using the deployment configuration.

---

### Step 3: Setup CI/CD

- Trigger pipeline on push to `main`  
- Build Docker image automatically  
- Deploy the updated image to EKS  

---

### Step 4: Logging

Use monitoring and logging tools such as:

- **AWS CloudWatch** – Collects logs and metrics from the EKS cluster  
- **EFK Stack (Elasticsearch, Fluentd, Kibana)** – Used for centralized logging and log visualization  

---
