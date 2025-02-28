# To create Taint and Tolerations, Node Affinity
```
kubectl taint node cka-cluster3-worker gpu=true:NoSchedule
```
# To remove taint for a node 
```
kubectl taint node cka-cluster3-worker gpu=true:NoSchedule-
```
# To set label for a node 
```
kubectl label node cka-cluster3-worker2 gpu:false
```
# T remove label for a node 
```
kubectl label node cka-cluster3-worker2 gpu-
```
# create pod.yml with toleration property 
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-toleration
  labels:
    name: nginx-toleration
spec:
  containers:
  - name: nginx-toleration
    image: nginx
    resources:
      limits:
        memory: "128Mi"
        cpu: "500m"
    ports:
      - containerPort: 8080
  tolerations:
  - key: "gpu"
    operator: "Equal"
    value: "true"
    effect: "NoSchedule"
```
---
```	
kubectl apply -f Pod.yaml 
kubectl describe pod nginx-toleration
kubectl get pod -o wide
kubectl describe node cka-cluster3-worker2 | grep "Taints"
```
# To set labels
```
kubectl label node cka-cluster3-worker2 gpu=false
```

# Node Affinity sample property
```yaml
 affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: disktype
            operator: In
            values:
            - ssd
      preferredDuringSchedulingIgnoredDuringExecution:
      - weight: 100
        preference:
          matchExpressions:
          - key: environment
            operator: In
            values:
            - production
```            