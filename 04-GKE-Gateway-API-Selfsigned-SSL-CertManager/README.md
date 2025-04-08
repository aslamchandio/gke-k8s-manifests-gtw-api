---
title: GKE with Kubernetes Gateway API - Load Balancer with self-signed SSL
description: Create GCP Application Load Balancer using GKE Kubernetes Gateway API, Self-signed SSL with Certificate manager
---

### Step-01-01: Create Regional Static IP
```t
# Create Regional Load Balancer IP

gcloud compute addresses create my-regional-pip \
--region us-central1 \
--network-tier STANDARD

# List IP Addresss    
gcloud compute addresses list

# List IP Addresss    
gcloud compute addresses describe my-regional-pip --region us-central1
```

### Step-01-01: Create Self-signed SSL certificates

```t
# Change Directory
cd self-signed-ssl

# Create your app1 key:
openssl genrsa -out app1.key 4096

# Create your app1 certificate signing request:
openssl req -new -key app1.key -out app1.csr -subj "/CN=app1.chandiolab.site"

# Create your app1 certificate:
openssl x509 -req -days 7300 -in app1.csr -signkey app1.key -out app1.crt
```

### Step-01-02: Create Kubernetes Secret
```t
# Change Directory
cd self-signed-ssl

# List Kubernetes Secrets
kubectl get secrets

# Create Kubernetes Secret
kubectl create secret tls myssl-secret \
    --cert=app1.crt \
    --key=app1.key

# List Kubernetes Secrets
kubectl get secrets
```

### Step-02-06: Deploy and Verify Resources
```t
# List Kubernetes Gateway Classes
kubectl get gatewayclass

# Deploy Kubernetes Resources
kubectl apply -f 03-GKE-Gateway-API-Selfsigned-SSL-k8sSecrets

# List Kubernetes Deployments
kubectl get deploy

# List Kubernetes Pods
kubectl get pods

# List Kubernetes Services
kubectl get svc

# List Kubernetes Gateways created using Gateway API
kubectl get gateway
kubectl get gtw

# Describe Gateway
kubectl describe gateway mygateway1-regional

# List HTTP Route
kubectl get httproute

# Verify Gateway is GCP GKE Console
Go to GKE Console -> Networking -> Gateways, Services & Ingress -> mygateway1-regional

# Verify GCP Cloud Load Balancer
Go to Cloud Load Balancers -> Review load balancer settings

# Access Application
https://<LB-IP>
https://app1.chandiolab.site

```