## Project Overview

The application is composed of three microservices:
- Front-End Service
- Product Catalog Service
- Order Processing Service

The goal is to automate the deployment and configuration of these services across development, testing, and production environments.


## Git Repository Setup

### Repository Creation
1. Create a Git repository to store all project files, including:
   - Git Repo
   - Dockerfiles
   - Kubernetes manifests

### Branching Strategy
1. **Branches**:
   - `Production` (Production)
   - `development` (Development)
   - `testing` (Testing)

![alt text](<images/Screenshot from 2024-08-02 15-01-20.png>)
---


2. **Merging Strategy**:
   - Develop features and fixes in the `development` branch.
   - Merge changes from `development` to `testing` for testing purposes.
   - After successful testing, merge `testing` changes into `Production` for production deployment.

## Dockerize Microservices

### Dockerfile Creation

1. Create a `Dockerfile` for each microservice:
   - `frontend/Dockerfile`
   - `catalog/Dockerfile`
   - `order-processing/Dockerfile`

2. **Dockerfile Example**:
```Dockerfile
FROM nginx:latest

COPY index.html /usr/share/nginx/html/

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```

### Image Building and Management
1. Build Docker images for each microservice:
```bash
docker build -t amaan00101/frontend:latest 
docker build -t amaan00101/catalog:latest
docker build -t amaan00101/order-processing:latest 
```

![alt text](<images/Screenshot from 2024-08-02 15-21-19.png>)

![alt text](<images/Screenshot from 2024-08-02 15-22-06.png>)

![alt text](<images/Screenshot from 2024-08-02 15-22-57.png>)
---


2. Push images to a container registry (e.g., Docker Hub):
```bash
docker push amaan00101/frontend:latest
docker push amaan00101/catalog:latest
docker push amaan00101/order-processing:latest
```

![alt text](<images/Screenshot from 2024-08-02 16-08-44.png>)
---


### Kubernetes Deployment

### Kubernetes Manifests
1. Create Kubernetes manifests for each microservice:
   - `frontend-deployment.yaml`
   - `catalog-deployment.yaml`
   - `order-processing-deployment.yaml`

2. Example `frontend-deployment.yaml`:
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: frontend
spec:
  replicas: 1
  selector:
    matchLabels:
      app: frontend
  template:
    metadata:
      labels:
        app: frontend
    spec:
      containers:
      - name: frontend
        image: amaan00101/frontend:latest
        ports:
        - containerPort: 80

---
apiVersion: v1
kind: Service
metadata:
  name: frontend-service
spec:
  type: NodePort
  selector:
    app: frontend
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
      nodePort: 30002
   
```

### Deployment to Kubernetes
1. Apply the Kubernetes manifests:
```bash
kubectl apply -f kubernetes/frontend-deployment.yaml
kubectl apply -f kubernetes/catalog-deployment.yaml
kubectl apply -f kubernetes/order-processing-deployment.yaml
```

![alt text](<images/Screenshot from 2024-08-02 16-59-24.png>)
---


2. Verify the deployment:
```bash
kubectl get pods
kubectl get services
```
![alt text](<images/Screenshot from 2024-08-02 17-43-53.png>)
---


### OutPut

![alt text](<images/Screenshot from 2024-08-02 17-19-39.png>)

![alt text](<images/Screenshot from 2024-08-02 17-24-42.png>)

![alt text](<images/Screenshot from 2024-08-02 17-24-59.png>)
---