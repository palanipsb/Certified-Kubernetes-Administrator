# Deploy DaemonSet, Job, CronJob

## DaemonSet - Create a DaemonSet

```
kubectl apply -f ds.yaml
```

### To check damonsets running in all and kube-system

kubectl get ds

kubectl get ds -A

kubectl get ds -n kube-system

### To check available clusters

kind get cluster

### switch to a cluster

kubectl config use-context kind-<cluster name>