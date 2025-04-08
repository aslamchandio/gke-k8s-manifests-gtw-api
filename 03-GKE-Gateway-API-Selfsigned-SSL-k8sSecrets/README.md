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