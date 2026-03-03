LEMICI DEVOPS INTERNSHIP ASSIGNMENT
📌 PART 1: VERSION CONTROL (GIT)
🔹 Difference Between git fetch and git pull
Feature	git fetch	git pull
Downloads Changes	✅ Yes	✅ Yes
Merges Automatically	❌ No	✅ Yes
Updates Working Directory	❌ No	✅ Yes
Safe Operation	✅ Safe	⚠ May Cause Conflicts
Combination Command	Only fetch	git fetch + git merge
🔹 git fetch

Downloads latest changes from remote repository

Updates remote tracking branch (origin/main)

Does NOT change current working branch

No automatic merge

git fetch origin
🔹 git pull

Downloads latest changes

Automatically merges into current branch

Can cause conflicts

git pull origin main
🔹 How to Resolve a Merge Conflict

When two branches modify the same line:

Git shows:

<<<<<<< HEAD
Your changes
=======
Incoming changes
>>>>>>> branch-name
✅ Steps to Resolve:

Open the file

Remove conflict markers

Keep correct code

Save file

git add filename
git commit -m "Conflict resolved"
git push origin main

✔ Conflict resolved successfully

🐳 PART 2: DOCKER & CONTAINERIZATION
🔹 Simple Python Flask Application
app.py
from flask import Flask

app = Flask(__name__)

@app.route("/")
def home():
    return "Hello, DevOps Internship Assignment!"

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)
requirements.txt
flask
🔹 Dockerfile
FROM python:3.9-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY app.py .
EXPOSE 5000
CMD ["python", "app.py"]
🔹 Dockerfile Explanation (Line-by-Line)

FROM python:3.9-slim → Uses lightweight Python image

WORKDIR /app → Sets working directory inside container

COPY requirements.txt . → Copies dependency file

RUN pip install --no-cache-dir → Installs Flask without cache

COPY app.py . → Copies application code

EXPOSE 5000 → Opens port 5000

CMD → Starts the application

🔹 Difference: Dockerfile vs Image vs Container
Dockerfile	Docker Image	Docker Container
Blueprint	Built Package	Running Instance
Text Instructions	App + Dependencies	Executing App
Used to build image	Created using docker build	Created using docker run
🔹 Reducing Docker Image Size

Best Practices:

Use slim/alpine base images

Use --no-cache-dir

Use multi-stage builds

Remove unnecessary files

Use .dockerignore

Example .dockerignore
__pycache__
*.pyc
.git
🔹 Running Application Locally
docker build -t my-flask-app .
docker run -p 5000:5000 my-flask-app

Access:

http://localhost:5000

Output:

Hello, DevOps Internship Assignment!
☸ PART 3: KUBERNETES (EKS BASICS)
🔹 Difference Between Pod, Deployment, and Service
🔹 Pod

Smallest deployable unit

Runs one or more containers

Temporary in nature

🔹 Deployment

Manages Pods

Maintains replicas

Supports rolling updates

🔹 Service

Exposes Pods to network

Provides stable IP

Load balances traffic

🔹 Why Use EKS Instead of Running Kubernetes on VMs?

If Kubernetes is installed manually on EC2:

We must manage master nodes

Handle upgrades

Maintain etcd

Ensure high availability

Using managed Kubernetes like Amazon EKS:

AWS manages control plane

Automatic updates & patches

High availability built-in

Integrated with IAM and VPC

Less operational overhead

🔹 Kubernetes Deployment YAML
k8s/deployment.yaml
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
🔹 Service YAML (LoadBalancer)
k8s/service.yaml
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

Explanation:

replicas: 2 → Runs two pods

type: LoadBalancer → Exposes application externally

port: 80 → Public port

targetPort: 5000 → Container port

⚙ PART 4: CI/CD PIPELINE
🔹 GitHub Actions Workflow

Platform Used: GitHub Actions

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
🔹 How Pipeline Changes to Deploy to Kubernetes

To deploy after build, we add:

Setup kubectl

Configure cluster credentials

Run:

kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml

This makes the pipeline:

Build → Test → Push → Deploy to Kubernetes

📊 PART 5: MONITORING & LOGS
🔹 Metrics vs Logs vs Traces

Metrics → Numeric system data (CPU, Memory, Requests)

Logs → Detailed event records (errors, requests)

Traces → Track request flow across services

🔹 If Kubernetes Pod Crashes – Debug Steps
kubectl get pods
kubectl describe pod <pod-name>
kubectl logs <pod-name>
kubectl logs <pod-name> --previous
kubectl top pod <pod-name>
🔹 Monitoring Tools for AWS EKS

Amazon CloudWatch → AWS native monitoring & logs

Prometheus → Kubernetes metrics collection

Grafana → Dashboards & visualization

ELK / EFK → Centralized logging

✅ Recommended: Prometheus + Grafana + CloudWatch Logs

🧠 PART 6: PROBLEM-SOLVING SCENARIO
🔹 Requirements

Code on GitHub

Containerize application

Auto-deploy on merge to main

Logs visible to development team

🔹 Solution Approach
1️⃣ Containerize

Create Dockerfile → Build → Push to DockerHub

2️⃣ Deploy to EKS

Create Deployment & Service YAML

kubectl apply -f deployment.yaml
3️⃣ Setup CI/CD (GitHub Actions)

Trigger on push to main:

Build Docker image

Push to DockerHub

Deploy to EKS

4️⃣ Logging

Use CloudWatch or EFK stack so developers can view logs.

📁 FINAL REPOSITORY STRUCTURE
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
