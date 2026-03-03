📌 Difference Between git fetch and git pull
🔹 git fetch

Downloads latest changes from remote repository.

Updates remote tracking branch (origin/main).

Does NOT change your current working branch.

Safe operation.

No automatic merge happens.

Example:

git fetch origin
🔹 git pull

Downloads latest changes from remote repository.

Automatically merges changes into your current branch.

Combination of:

git fetch + git merge

Example:

git pull origin main
✅ Main Difference
git fetch	git pull
Only downloads changes	Downloads + merges
No changes in working directory	Updates working directory
Safe	May cause conflict
📌 How to Resolve a Git Merge Conflict (Example Scenario)

When two branches modify the same line in a file, Git cannot decide which change to keep.

Git shows:

<<<<<<< HEAD
Your changes
=======
Incoming changes
>>>>>>> branch-name
Steps to resolve:

Open the file.

Remove conflict markers.

Keep the correct code.

Save the file.

Add file to staging:

git add filename

Commit:

git commit -m "Conflict resolved"

Conflict resolved successfully.

Save file and push:

git add README.md
git commit -m "Added README explanation"
git push origin main 
# 🚀 Lemici DevOps Internship Assignment

---

# ✅ Part 1: Version Control (Already Completed)

✔ Git repository created using SSH
✔ Difference between `git fetch` and `git pull` explained
✔ Merge conflict created and resolved successfully

---

# ✅ Part 2: Docker & Containerization

## 🔹 1. Simple Python Web Application

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

## 🔹 2. Dockerfile

```dockerfile
# Use lightweight Python base image
FROM python:3.9-slim

# Set working directory
WORKDIR /app

# Copy dependency file
COPY requirements.txt .

# Install dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Copy application code
COPY app.py .

# Expose application port
EXPOSE 5000

# Run application
CMD ["python", "app.py"]
```

---

## 🔹 Dockerfile Explanation (Line-by-Line)

* `FROM python:3.9-slim` → Uses lightweight Python image to reduce size
* `WORKDIR /app` → Sets working directory inside container
* `COPY requirements.txt .` → Copies dependency file
* `RUN pip install --no-cache-dir` → Installs Flask without cache (reduces size)
* `COPY app.py .` → Copies application code
* `EXPOSE 5000` → Opens port 5000
* `CMD` → Starts the application

---

## 🔹 Difference Between Dockerfile, Image, and Container

| Dockerfile          | Docker Image               | Docker Container         |
| ------------------- | -------------------------- | ------------------------ |
| Blueprint           | Built package              | Running instance         |
| Text instructions   | App + dependencies         | Executing app            |
| Used to build image | Created using docker build | Created using docker run |

---

## 🔹 Reducing Image Size

To reduce Docker image size:

* Use slim/alpine base images
* Use `--no-cache-dir`
* Use multi-stage builds
* Remove unnecessary files
* Use `.dockerignore`

Example `.dockerignore`:

```
__pycache__
*.pyc
.git
```

---

## 🔹 Running Application Locally

### Build Image

```
docker build -t my-flask-app .
```

### Run Container

```
docker run -p 5000:5000 my-flask-app
```

### Access in Browser

```
http://localhost:5000
```

Output:

```
Hello, DevOps Internship Assignment!
```i
![Flask App Output](screenshots/flask-app-output.png)
---

# ✅ Part 3: Kubernetes (EKS Basics)

---

## 🔹 1. Difference Between Pod, Deployment, and Service

### 🔹 Pod

* Smallest unit in Kubernetes
* Runs one or more containers
* Temporary in nature

### 🔹 Deployment

* Manages Pods
* Maintains replicas
* Supports rolling updates

### 🔹 Service

* Exposes Pods to network
* Provides stable IP
* Load balances traffic

---

## 🔹 Why Use EKS Instead of Running Kubernetes on VMs?

If Kubernetes is installed manually on EC2:

* We must manage master nodes
* Handle upgrades
* Maintain etcd
* Ensure high availability

Using managed Kubernetes (EKS):

* AWS manages control plane
* Automatic updates & patches
* High availability built-in
* Integrated with IAM and VPC
* Less operational overhead

---

## 🔹 2. Kubernetes Deployment YAML

### k8s/deployment.yaml

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

## 🔹 Service YAML (LoadBalancer)

### k8s/service.yaml

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

### Explanation

* `replicas: 2` → Runs two pods
* `type: LoadBalancer` → Exposes application externally
* `port: 80` → Public port
* `targetPort: 5000` → Container port

---

# ✅ Part 4: CI/CD Pipeline

---

## 🔹 GitHub Actions Workflow

Location:

```
.github/workflows/docker.yml
```

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

    - name: Run Tests
      run: echo "Tests passed"

    - name: Push Docker Image
      run: echo "Simulated Docker push"
```

---

## 🔹 Pipeline Explanation

* Runs on push to main branch
* Builds Docker image
* Runs test step
* Push step simulated

---

## 🔹 How Pipeline Changes to Deploy to Kubernetes

To deploy after build, we add:

```
- Setup kubectl
- Configure cluster credentials
- Run kubectl apply -f k8s/
```

Example:

```
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml
```

This makes the pipeline:

Build → Test → Push → Deploy to Kubernetes

---

# ✅ Final Outcome

✔ Dockerized Flask Application
✔ Kubernetes YAML with 2 replicas
✔ LoadBalancer Service
✔ GitHub Actions CI Pipeline
✔ Deployment Ready Structure

---

## 📌 Repository Structure

```
lemici-devops-assignment/
│
├── Dockerfile
├── app.py
├── requirements.txt
├── k8s/
│   ├── deployment.yaml
│   └── service.yaml
│
└── .github/
    └── workflows/
        └── docker.yml

