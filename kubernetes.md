#  PingPong App on Kubernetes (Minikube)

This project deploys a full-stack **PingPong application** on a local Kubernetes cluster using **Minikube**. It includes:

* Frontend
* Backend
* PostgreSQL (StatefulSet)
* Redis
* Ingress for routing

---

##  Project Structure

[Go to: Manifest](./manifest/)
```
.
├── backend
│   ├── backend-deployment.yaml
│   └── backend-service.yaml
├── frontend
│   ├── frontend-deployment.yaml
│   └── frontend-service.yaml
├── ingress.yaml
├── postgres
│   ├── postgres-secrets.yaml
│   ├── postgres-service.yaml
│   ├── postgres-statefulset.yaml
│   └── postgres-volume.yaml
└── redis
    ├── redis-deployment.yaml
    └── redis-service.yaml
```

---

##  Prerequisites

Make sure you have installed:

* Kubernetes CLI (`kubectl`)
* Minikube
* Docker

---

##  Step 1: Start Minikube

```bash
minikube start
```

Enable ingress controller:

```bash
minikube addons enable ingress
```

---

##  Step 2: Configure Hosts File

Get your Minikube IP:

```bash
minikube ip
```

Edit your `/etc/hosts` file:

```bash
sudo nano /etc/hosts
```

Add this line:

```
<minikube-ip> pingpong.local
```

Example:

```
192.168.49.2 pingpong.local
```

---

##  Step 3: Deploy All Resources

Apply everything in order:

```bash
kubectl apply -f postgres/
kubectl apply -f redis/
kubectl apply -f backend/
kubectl apply -f frontend/
kubectl apply -f ingress.yaml
```

---

##  Step 4: Verify Deployment

Check pods:

```bash
kubectl get pods
```

Check services:

```bash
kubectl get svc
```

Check ingress:

```bash
kubectl get ingress
```

---

##  Step 5: Access Application

Open your browser and go to:

```
http://pingpong.local
```

---

##  Architecture Overview

```
User → Ingress → Frontend → Backend → PostgreSQL
                                ↘ Redis
```

* **Ingress** routes traffic to frontend
* **Frontend** communicates with backend
* **Backend** connects to:

  * PostgreSQL (persistent storage)
  * Redis (caching)

---

##  Useful Commands

Restart a deployment:

```bash
kubectl rollout restart deployment <name>
```

View logs:

```bash
kubectl logs -f <pod-name>
```

Delete all resources:

```bash
kubectl delete -f .
```

---

##  Troubleshooting

### Ingress not working?

```bash
minikube addons enable ingress
kubectl get pods -n ingress-nginx
```

### Host not resolving?

Make sure `/etc/hosts` is correctly updated.

### Pods not starting?

```bash
kubectl describe pod <pod-name>
kubectl logs <pod-name>
```

---

##  Notes

* PostgreSQL is deployed as a **StatefulSet** with persistent storage.
* Secrets are used for database credentials.
* Redis is used for caching.
* Images used:

  * `amitgiri13/pingpong-frontend:1.0.0`
  * `amitgiri13/pingpong-backend:1.0.0`

---

