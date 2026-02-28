# 🍦 IceCream Application – DevOps Deployment Project
## 📌 Project Overview

This project demonstrates the containerization, orchestration, load testing, monitoring, and fault tolerance validation of the IceCream web application using modern DevOps tools.

The application is built with:

* Node.js (Express)
* MongoDB
* EJS templating engine
* Containerized using Docker
* Orchestrated using Kubernetes (Minikube)

The objective was to migrate a locally running application into a fully containerized and orchestrated infrastructure while validating performance and reliability under load.

## 🏗️ System Architecture
User
  ↓
NodePort Service
  ↓
IceCream Pods (ReplicaSet - 2 replicas)
  ↓
MongoDB Pod

The application runs as multiple replicas for high availability.

MongoDB runs as a separate pod within the cluster.

The NodePort service exposes the application externally.

## 📂 Project Structure
IceCream/
│
├── Dockerfile
├── docker-compose.yml
├── package.json
├── index.js
├── config/
├── routes/
├── views/
│
├── k8s/
│   ├── deployment.yaml
│   ├── service.yaml
│   ├── mongo-deployment.yaml
│
└── README.md
## 🚀 Running the Application Locally
1️⃣ Install Dependencies
---------------------------------------
npm install
----------------------------------------
2️⃣ Start MongoDB Locally

Ensure MongoDB is running:

mongod
3️⃣ Start the Application
npm start

Access the app at:

http://localhost:3000
### 🐳 Running with Docker
1️⃣ Build Docker Image
docker build -t icecream-app .
2️⃣ Run Container
docker run -p 3000:3000 icecream-app
3️⃣ Verify Running Containers
docker ps

Example output:

CONTAINER ID   IMAGE           PORTS                    NAMES
abc123         icecream-app    0.0.0.0:3000->3000/tcp   icecream

Access:

http://localhost:3000
## 🐳 Docker Compose (Multi-Container Setup)

Start both application and MongoDB:

docker-compose up --build

Verify:

docker ps

Stop:

docker-compose down
## ☸️ Kubernetes Deployment (Minikube)
1️⃣ Start Minikube
minikube start
2️⃣ Point Docker to Minikube
minikube docker-env | Invoke-Expression
3️⃣ Build Image Inside Minikube
docker build -t icecream-app .
4️⃣ Deploy to Kubernetes
kubectl apply -f k8s/

Verify pods:

kubectl get pods

Verify services:

kubectl get services
5️⃣ Access Application

Port forward:

kubectl port-forward service/icecream-service 8080:80

Access:

http://127.0.0.1:8080
## 📊 Load Testing

Load testing was performed using ApacheBench (ab).

Command Used:
ab -n 30000 -c 500 http://127.0.0.1:8080/
Test Configuration:

30,000 total requests

500 concurrent users

Results:

Concurrency Level: 500

Complete Requests: 30000

Failed Requests: 0

Requests per second: 47.07

Average Response Time: 10621 ms

The system handled high concurrency without crashing or failing requests.

## 📈 Monitoring and Observability

Metrics-server was enabled:

minikube addons enable metrics-server

Resource monitoring:

kubectl top pods

Example observed usage:

Pod	CPU	Memory
IceCream	4–6m	~42Mi
MongoDB	~10m	~175Mi

The system remained stable under load.

## 💥 Fault Tolerance Validation

To test Kubernetes self-healing:

kubectl delete pod icecream-64cd59c79b-9wzvk

Observed behavior:

Pod terminated

New pod automatically created

Replica count maintained

No downtime observed

This confirms Kubernetes deployment controller and ReplicaSet functionality.

## 🔄 Scaling (Optional Enhancement)

Manual scaling:

kubectl scale deployment icecream --replicas=4

Auto-scaling (if configured):

kubectl autoscale deployment icecream --cpu-percent=50 --min=2 --max=6
## 🛠️ Challenges Encountered

ImagePullBackOff due to local image scope

Networking limitations with Minikube on Windows

Metrics-server initialization delay

Increased response time under heavy load

## 📌 Future Improvements

Implement Horizontal Pod Autoscaler

Configure liveness and readiness probes

Add resource limits to deployments

Implement CI/CD pipeline

Use persistent volumes for MongoDB

## 🧪 Commands Reference
Docker
docker build -t icecream-app .
docker ps
docker-compose up
Kubernetes
kubectl apply -f k8s/
kubectl get pods
kubectl get services
kubectl describe service icecream-service
kubectl delete pod 64cd59c79b-9wzvk
kubectl top pods
## 📚 Technologies Used

Node.js

MongoDB

Docker

Docker Compose

Kubernetes

Minikube

ApacheBench

## ✅ Project Status

✔ Containerized
✔ Deployed to Kubernetes
✔ Load tested (500 concurrent users)
✔ Monitored via metrics-server
✔ Fault tolerance validated

## ⭐ Final Notes

This project demonstrates practical implementation of DevOps principles including containerization, orchestration, scalability, resilience, and performance evaluation.
