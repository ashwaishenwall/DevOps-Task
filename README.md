# Task 1:- Deploy Simple Node.js Application using Docker

This repository contains a lightweight Node.js application built with Express. The application serves a basic "Hello, World!" message and is fully containerized using Docker for streamlined deployment.

---

## Features

- **Node.js & Express**: Implements a simple HTTP server.
- **Dockerized Deployment**: Easily build and run using Docker.
- **Lightweight Image**: Based on `node:18-alpine` for minimal resource usage.

---

## Prerequisites

Ensure the following tools are installed:

- [Node.js](https://nodejs.org/) (optional if running via Docker)
- [Docker](https://www.docker.com/)

---

## Project Structure

```plaintext
.
├── app.js            # Main application file
├── Dockerfile        # Docker configuration for containerization
├── package.json      # Project metadata and dependencies
└── package-lock.json # Dependency tree (auto-generated)
```
## Setup and Usage
#### Cloning the Repository
```
git clone https://github.com/ashwaishenwall/DevOps-Task.git
cd Task-1
```
## Running Locally (Without Docker)
#### Install Dependencies:
```
npm install
```
#### Start the Application:
```
npm start
```
#### Access the Application: 
Open your browser and navigate to http://localhost:3000.

## Running with Docker
#### Build the Docker Image:
```
docker build -t simple-node-app .
```
#### Run the Docker Container:
```
docker run -p 3000:3000 simple-node-app
```
#### Access the Application: Visit http://localhost:3000 in your browser.

## Output
When you access the root endpoint (/), you will receive the following response:
```
Hello, World!
```
#
# Task 2:- Web App Deployment Using Kubernetes

This repository contains a Kubernetes Deployment manifest (app.yaml) to deploy a simple web application using the Nginx image. The Deployment ensures high availability, resource management, and readiness/liveness probes for robust operation.

## Overview

The `app.yaml` file defines a Kubernetes Deployment for a web application using Nginx as a container. It deploys 3 replicas of the web application and sets resource limits and probes to ensure availability and performance.

## Contents of `app.yaml`

### 1. **Deployment Metadata**

```yaml
metadata:
  name: web-app-deployment
  labels:
    app: web-app
```
**name:** The name of the deployment is web-app-deployment.

**labels:** Labels are used to identify and group related Kubernetes resources. In this case, the label `app: web-app` is assigned to the deployment.

### 2. Deployment Specification
#
```
spec:
  replicas: 3
  selector:
    matchLabels:
      app: web-app
  template:
    metadata:
      labels:
        app: web-app
```
**replicas:** Specifies the desired number of pod replicas. In this case, we want 3 replicas of the web app.

**selector:** This defines how the deployment identifies which pods to manage. Here, it selects pods with the label app: web-app.

**template:** Describes the pod template used to create the pods. It includes metadata (labels) and the container configuration.

### 3. Container Configuration
#
```
containers:
  - name: nginx
    image: nginx:latest
    ports:
      - containerPort: 80
    resources:
      requests:
        memory: "64Mi"
        cpu: "250m"
      limits:
        memory: "128Mi"
        cpu: "500m"
    livenessProbe:
      httpGet:
        path: /healthz
        port: 80
      initialDelaySeconds: 10
      periodSeconds: 10
    readinessProbe:
      httpGet:
        path: /healthz
        port: 80
      initialDelaySeconds: 5
      periodSeconds: 5
```

**name:** The container name is `nginx`, and the container image used is nginx:latest.

**ports:** The container listens on port 80.

**resources:** The container has resource requests and limits to manage CPU and memory allocation.

**requests:** Minimum resources required by the container.

**limits:** Maximum resources that the container can use.

**livenessProbe:** A probe to check if the application is still running. It checks the `/healthz` endpoint every 10 seconds with an initial delay of 10 seconds.

**readinessProbe:** A probe to check if the application is ready to accept traffic. It checks the `/healthz` endpoint every 5 seconds with an initial delay of 5 seconds.

### 4. QoS Class
#
```
qosClass: Guaranteed
```

**qosClass:** This ensures that the pod gets guaranteed quality of service based on the requested and limited resources.

### Deployment Steps
#### **Step 1:** Apply the Deployment YAML

To create the deployment, use the following command to apply the app.yaml file:
```
kubectl apply -f app.yaml
```

This command will create the deployment and the associated pods on your Kubernetes cluster.
#
#### **Step 2:** Verify the Deployment
You can check the status of the deployment using the following command:
```
kubectl get deployments
```

This will show the web-app-deployment and its status, including the number of replicas running.

#### **Step 3:** Check Pod Status
#
To verify that the pods are running successfully, use the following command:
```
kubectl get pods
```
This will display the pods created by the deployment. They should have the label `app=web-app` and show `Running` status.

#### **Step 4:** Expose the Web App
If you want to expose the web application externally, you can create a Service to expose the pods. For example, use the following command to create a LoadBalancer service:
```
kubectl expose deployment web-app-deployment --type=LoadBalancer --name=web-app-service
```

This will create a LoadBalancer service and provide an external IP to access the web app.

#### **Step 5:** Verify the Service
To check the service and get the external IP, use:
```
kubectl get services
```
Look for the web-app-service in the list. Once the external IP is assigned, you can access the web app using the IP.

#### **Step 6:** Clean Up
When you're done, you can delete the deployment and service with:
```
kubectl delete deployment web-app-deployment
kubectl delete service web-app-service
```
# Task 3:- 
