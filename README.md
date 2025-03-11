[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/97WR5HaV)

# Flask PostgreSQL Kubernetes Deployment

This project demonstrates deploying a Flask application with PostgreSQL database on Kubernetes using Minikube.

## Prerequisites

- Docker
- Minikube
- kubectl
- Python 3.9+

## Project Structure

```
k8s-flask-app/
│── manifests/
│   │── deployment/
│   │   │── flask-deployment.yaml
│   │   │── postgres-deployment.yaml
│   │── service/
│   │   │── flask-service.yaml
│   │   │── postgres-service.yaml
│   │── configmap/
│   │   │── postgres-configmap.yaml
│   │── secret/
│   │   │── postgres-secret.yaml
│── app/
│   │── Dockerfile
│   │── requirements.txt
│   │── app.py
│── README.md
```

## Setup Instructions

1. Start Minikube:
   ```bash
   minikube start
   ```

2. Enable the Minikube Docker daemon:
   ```bash
   eval $(minikube docker-env)
   ```

3. Build the Flask application Docker image:
   ```bash
   cd app
   docker build -t flask-app:latest .
   cd ..
   ```

4. Apply Kubernetes manifests:
   ```bash
   kubectl apply -f manifests/configmap/
   kubectl apply -f manifests/secret/
   kubectl apply -f manifests/deployment/
   kubectl apply -f manifests/service/
   ```

5. Wait for all pods to be ready:
   ```bash
   kubectl get pods -w
   ```

## Accessing the Application

1. Get the URL for the Flask service:
   ```bash
   minikube service flask-service --url
   ```

2. Test the application:
   - Visit the root endpoint: `<service-url>/`
   - Check database connection: `<service-url>/db-test`
   - Health check: `<service-url>/health`

## Cleanup

To delete all resources:
```bash
kubectl delete -f manifests/service/
kubectl delete -f manifests/deployment/
kubectl delete -f manifests/configmap/
kubectl delete -f manifests/secret/
minikube stop
```

## Notes

- The PostgreSQL password is base64 encoded in the secret manifest
- The application uses environment variables for database configuration
- The Flask application is deployed with 2 replicas for high availability
- PostgreSQL data is persisted using a PersistentVolumeClaim
