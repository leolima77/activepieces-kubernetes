# Activepieces on Kubernetes

This repository contains a basic example of how to deploy [Activepieces](https://www.activepieces.com/) on a Kubernetes cluster with a PostgreSQL and Redis backend.

We assume you have postgres and redis online on some server.

## Stack Overview

- **Activepieces**: Low-code automation tool
- **PostgreSQL**: Persistent storage for workflows and execution logs
- **Redis**: Caching and job queue backend
- **Kubernetes**: Orchestrator for running containers
- **NGINX Ingress**: Handles external HTTP traffic routing

---

## Deployment Overview

The provided YAML will:

1. Create a namespace called `activepieces`
2. Deploy the Activepieces container
3. Configure environment variables for PostgreSQL and Redis
4. Expose the application via a Kubernetes `Service`
5. Define an `Ingress` rule using NGINX for external access

---

## 🛠️ Prerequisites

- A running Kubernetes cluster (e.g., Minikube, AKS, EKS, GKE)
- NGINX Ingress Controller installed
- PostgreSQL with database 'ActivePieces' created previously and Redis accessible within your cluster
- A domain (or local DNS entry) pointing to your Ingress IP

---

## Environment Variables (Redacted Example)

```env
DATABASE_URL=postgres://username:password@postgres-host:2032/activepieces

# PostgreSQL
AP_POSTGRES_HOST=postgres-host
AP_POSTGRES_PORT=2032
AP_POSTGRES_USERNAME=username
AP_POSTGRES_PASSWORD=password
AP_POSTGRES_DATABASE=activepieces

# Redis
REDIS_HOST=redis-host
REDIS_PORT=2092
REDIS_PASSWORD=password
AP_REDIS_HOST=redis-host
AP_REDIS_PORT=2092
AP_REDIS_PASSWORD=password

# App
PIECES_API_URL=http://127.0.0.1
AP_ENCRYPTION_KEY=your-encryption-key
AP_JWT_SECRET=your-jwt-secret
AP_FRONTEND_URL=http://activepieces.example.com
AP_ENVIRONMENT=prod
AP_EXECUTION_MODE=UNSANDBOXED
AP_WEBHOOK_TIMEOUT_SECONDS=30
AP_TRIGGER_DEFAULT_POLL_INTERVAL=1
```

> **Important:** Replace the credentials and URLs above with your actual secrets and endpoints.

---

## Accessing the App

Once deployed, you can access Activepieces via the domain configured in the Ingress rule:

```
http://activepieces.example.com
```

> Make sure DNS resolves to your cluster’s ingress IP.

---

## Example Deployment

Apply the configuration with:

```bash
kubectl apply -f activepieces-deployment.yaml
```

Verify everything is running:

```bash
kubectl get pods -n activepieces
kubectl get svc -n activepieces
kubectl get ingress -n activepieces
```