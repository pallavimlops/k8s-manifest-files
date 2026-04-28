# Secrets — Execution Steps

## Pre-requisite
Make sure ConfigMap is running from 01-configmap folder!
kubectl get configmaps

---

## Type 1 — Opaque (passwords, API keys)

kubectl create secret generic app-secret \
  --from-literal=DB_PASSWORD=password123 \
  --from-literal=API_KEY=my-secret-api-key

### Verify Secret
kubectl get secrets
kubectl describe secret app-secret

### Apply Deployment
kubectl apply -f deployment.yaml

### Verify all env variables injected
kubectl get pods
kubectl exec -it <pod-name> -- env | grep -E "DB_URL|APP_ENV|DB_PASSWORD|API_KEY"

### See difference between ConfigMap and Secret
kubectl get configmap app-config -o yaml
# Values visible in plain text!

kubectl get secret app-secret -o yaml
# Values are base64 encoded!

### Decode Secret value
echo "cGFzc3dvcmQxMjM=" | base64 --decode
# output → password123

---

## Type 2 — dockerconfigjson
Kubernetes cannot pull the image unless we give credentials.
That is what dockerconfigjson Secret is for!

### For AWS ECR
kubectl create secret docker-registry ecr-secret \
  --docker-server=550651695287.dkr.ecr.us-east-1.amazonaws.com \
  --docker-username=AWS \
  --docker-password=$(aws ecr get-login-password --region us-east-1)

### For GitLab Registry
kubectl create secret docker-registry gitlab-secret \
  --docker-server=registry.gitlab.com \
  --docker-username=<your-gitlab-username> \
  --docker-password=<your-access-token> \
  --docker-email=<your-email>

### Verify
kubectl get secrets
kubectl describe secret ecr-secret

---

## Type 3 — TLS (HTTPS certificates)

# Generate self signed certificate for demo
openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
  -keyout tls.key \
  -out tls.crt \
  -subj "/CN=myapp.example.com"

# Create TLS secret
kubectl create secret tls my-tls-secret \
  --cert=tls.crt \
  --key=tls.key

### Verify
kubectl get secrets
kubectl describe secret my-tls-secret

---

## See all Secret types at end

kubectl get secrets

Expected output:
NAME                  TYPE
app-secret            Opaque
ecr-secret            kubernetes.io/dockerconfigjson
my-tls-secret         kubernetes.io/tls
