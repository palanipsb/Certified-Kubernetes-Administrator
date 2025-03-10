# Deployment and Replication

```linux
kubectl apply -f <yaml file name>
```

## To see resource

```linux
kubectl get resource/resource_name

kubectl get rc/nginx-rc
```

## To delete all pods under a resource

```linux
kubectl delete rc/nginx-rc
```

## To Delete one pod

```linux
kubectl delete pod <pod_name>
```

## To edit replicas or other items in pods: declarative approach

```linux
kubectl edit resource/resource_name

kubectl edit rc/nginx-rc
```

## To edit replicas in imperative approach

```linux
kubectl scale --replicas=10 resource/resource_name

kubectl scale --replicas=10 ReplicaSet/myreplicaset-nginx
```

### after Deployment

```linux
kubectl get deploy
```

### To see all services running

```linux
kubectl get all
```

### To set new image for conntainer

```linux
kubectl set image resource/resource_name container_name=<new image version>

kubectl set image deploy/nginx-deploy nginx-container=nginx:1.9.1
```

### To see changes

```linux
kubectl describe resource/resource_name

kubectl describe deploy/nginx-deploy
```

### To see rollout history - version changes

```linux
kubectl rollout history resource/resource_name

kubectl rollout history deploy/nginx-deploy
```

### To undo rollout

```linux
kubectl rollout undo resource/resource_name

kubectl rollout undo deploy/nginx-deploy
```
