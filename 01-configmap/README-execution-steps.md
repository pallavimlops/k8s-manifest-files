# ConfigMap — Execution Steps

## keep the Files in a folder. 
configmap.yaml   → stores all config
deployment.yaml  → app reads from ConfigMap
service.yaml     → expose the app

## Step 1 — Apply all files
kubectl apply -f configmap.yaml
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml

## Step 2 — Verify everything
kubectl get configmaps
kubectl get deployments
kubectl get pods
kubectl get svc

## Step 3 — Verify ConfigMap values injected into Pod
kubectl get pods
kubectl exec -it <pod-name> -- env | grep -E "DB_URL|APP_ENV|MAX_CONNECTIONS|APP_PORT"

Expected output:
DB_URL=mysql-service:3306
APP_ENV=production
MAX_CONNECTIONS=100
APP_PORT=5000

## Step 4 — Show power of ConfigMap
kubectl edit configmap app-config
# Change APP_ENV from production to staging

kubectl rollout restart deployment telecom-app

kubectl exec -it <new-pod-name> -- env | grep APP_ENV

Expected output:
APP_ENV=staging
