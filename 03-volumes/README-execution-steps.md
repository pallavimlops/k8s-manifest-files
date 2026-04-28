# Volumes — Execution Steps

## Files in a folder
01-storageclass.yaml         → StorageClass
02-pv.yaml                   → Persistent Volume
03-pvc.yaml                  → Persistent Volume Claim
04-deployment-with-pvc.yaml  → Deployment using PVC

## StorageClass
kubectl get storageclass
kubectl apply -f 03-storageclass.yaml
kubectl get storageclass

## PV
kubectl apply -f 04-pv.yaml
kubectl get pv
# Status = Available → waiting for PVC!

## PVC
kubectl apply -f 05-pvc.yaml
kubectl get pvc
# Status = Bound → matched to PV!
kubectl get pv
# PV status changed from Available to Bound!

## Deployment with PVC
kubectl apply -f 06-deployment-with-pvc.yaml
kubectl get pods

## Seedata survives Pod restart!
kubectl exec -it <pod-name> -- sh -c "echo 'important data' > //app/data/test.txt"
kubectl exec -it <pod-name> -- cat //app/data/test.txt
kubectl delete pod <pod-name>
kubectl get pods
kubectl exec -it <new-pod-name> -- cat //app/data/test.txt
# Data survived Pod restart!
