## Execution Steps

# Create a manifest file and save it
vi statefulset.yaml

kubectl apply -f statefulset.yaml

# Watch Pods starting in ORDER
kubectl get pods --watch
# mysql-0 starts first → only then mysql-1

# Check fixed Pod names
kubectl get pods
# mysql-0, mysql-1 — names never change!

# Check each Pod has its own PVC
kubectl get pvc
# mysql-data-mysql-0 → each Pod gets own PVC!

# Delete Pod and see it comes back with SAME name
kubectl delete pod mysql-0
kubectl get pods --watch
# mysql-0 comes back with same name!

# Describe StatefulSet
kubectl describe statefulset mysql