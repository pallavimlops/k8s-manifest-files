## Execution Steps

```bash
# Enable metrics-server first
minikube addons enable metrics-server

# Create a hpa manifest file and save it
kubectl apply -f hpa.yaml

# Check HPA
kubectl get hpa
# TARGETS shows current CPU usage

# Generate load to trigger scaling
kubectl run load-generator \
  --image=busybox \
  --restart=Never \
  -- /bin/sh -c "while true; do wget -q -O- http://telecom-app-service; done"

kubectl run load-generator --image=busybox --restart=Never -- /bin/sh -c "while true; do wget -q -O- http://telecom-app-service; done"


# Watch HPA scaling Pods automatically!
kubectl get hpa --watch
kubectl get pods --watch


# Stop load generator
kubectl delete pod load-generator

# Watch HPA scale down
kubectl get hpa --watch
# Pods reduce automatically!