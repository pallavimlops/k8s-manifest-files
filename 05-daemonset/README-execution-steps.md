## Execution Steps

```bash
# First s existing DaemonSets
kubectl get daemonsets -n kube-system
# kube-proxy runs on every node!

Create a daemonset.yaml & save it
vi daemonset.yaml

kubectl apply -f daemonset.yaml

# Verify DaemonSet
kubectl get daemonsets
# DESIRED = 1 because Minikube has 1 node
# In real cluster with 5 nodes → DESIRED = 5!

# Check Pod running on node
kubectl get pods -o wide

# Check logs
kubectl logs <daemonset-pod-name>

# Verify data on node
minikube ssh
cat /tmp/collected-logs/collected.log
exit

# Try to scale DaemonSet — it won't work!
kubectl scale daemonset log-collector --replicas=3
# Error: DaemonSets don't support scaling

# Describe DaemonSet
kubectl describe daemonset log-collector
