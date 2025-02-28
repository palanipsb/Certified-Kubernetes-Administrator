# Static Pod, Manual Schedling, Labels, Selectors

## To get into control plane
```
docker exec -it cka-cluster3-control-plane bash
```
## Static pod yaml files can be found below, To troubeshoot control plan node, we can check yaml files in below directory
```
cd /etc/kubernetes/manifests/
```
# Labeles
## To query pods with label name
```
kubectl get pods -l app=frontend

```
In Kubernetes, Static Pods, Manual Scheduling, Labels, and Selectors are important concepts that help you manage and organize your workloads. Let’s break down each of these in detail.

# 1. Static Pods
## What are Static Pods?
    Static Pods are Pods that are managed directly by the kubelet on a specific node, without the involvement of the Kubernetes API server.
    They are defined by placing Pod manifest files in a designated directory (e.g., /etc/kubernetes/manifests) on the node.
    The kubelet periodically checks this directory and ensures that the Pods defined in the manifest files are running.

## Key Features
    Node-Specific: Static Pods are tied to the node where their manifest files are located.
    No API Server Involvement: They are not managed by the Kubernetes control plane (e.g., no ReplicaSet or Deployment).
    Useful for Bootstrapping: Often used to run critical system components (e.g., kube-apiserver, kube-scheduler, kube-controller-manager) during cluster setup.

## How to Create a Static Pod
    Create a Pod manifest file (e.g., static-pod.yaml):
```
apiVersion: v1
kind: Pod
metadata:
  name: static-web
  labels:
    app: static-web
spec:
  containers:
  - name: nginx
    image: nginx
    ports:
    - containerPort: 80
```
    Place the manifest file in the kubelet’s static Pod directory (e.g., /etc/kubernetes/manifests):

```
sudo cp static-pod.yaml /etc/kubernetes/manifests/
```
    The kubelet will automatically create and manage the Pod.

## Commands to Manage Static Pods
    1. List Static Pods:
```
kubectl get pods
```
    (Static Pods will appear with the node name as a prefix, e.g., static-web-node1.)
    
    2. Delete a Static Pod:
Remove the manifest file from the static Pod directory:
```
sudo rm /etc/kubernetes/manifests/static-pod.yaml
```
# 2. Manual Scheduling
    What is Manual Scheduling?
    By default, Kubernetes uses the kube-scheduler to assign Pods to nodes based on resource availability, constraints, and policies.
    Manual Scheduling allows you to bypass the scheduler and explicitly assign a Pod to a specific node.

1. How to Manually Schedule a Pod
    You can manually schedule a Pod by setting the nodeName field in the Pod’s specification.
    Example YAML for Manual Scheduling
```
apiVersion: v1
kind: Pod
metadata:
  name: manual-schedule-pod
spec:
  containers:
  - name: nginx
    image: nginx
  nodeName: node-01  # Explicitly assign this Pod to node-01
```
## Use Cases
    1. Testing: Assigning Pods to specific nodes for testing purposes.
    2. Specialized Workloads: Running workloads that require specific hardware or configurations available only on certain nodes.
    3. Limitations
    If the specified node is unavailable, the Pod will remain in a Pending state.
    Manual scheduling bypasses the scheduler’s optimizations and policies.

# 3. Labels
    What are Labels?
    Labels are key-value pairs attached to Kubernetes objects (e.g., Pods, Services, Deployments).
    They are used to organize and select subsets of objects.

## Key Features
    1. Flexible: You can define any key-value pair that makes sense for your use case.
    2. Queryable: Labels can be used to filter and select objects.
    3. Immutable: Once an object is created, its labels cannot be changed (though you can delete and recreate the object).

    Example Labels
```
metadata:
  labels:
    app: frontend
    environment: production
    tier: web
```
    How to Use Labels
    Add Labels to a Pod:
```
apiVersion: v1
kind: Pod
metadata:
  name: labeled-pod
  labels:
    app: frontend
    environment: dev
spec:
  containers:
  - name: nginx
    image: nginx

```

## Query Objects by Label:


```
kubectl get pods -l app=frontend
```
# 4. Selectors
    What are Selectors?
    Selectors are used to filter and select objects based on their labels.
    They are commonly used in Deployments, Services, and ReplicaSets to define which Pods they should manage.

## Types of Selectors
    1. Equality-Based Selectors:
        Match objects with labels that have exact key-value pairs.
        Example:
```
selector:
  matchLabels:
    app: frontend
```
    2. Set-Based Selectors:

        Match objects based on a set of values or conditions.
        Example:
```
selector:
  matchExpressions:
    - {key: environment, operator: In, values: [dev, staging]}
```
    Example Use of Selectors
    Service Selecting Pods:
```
apiVersion: v1
kind: Service
metadata:
  name: frontend-service
spec:
  selector:
    app: frontend
  ports:
  - protocol: TCP
    port: 80
    targetPort: 8080
```
    Deployment Selecting Pods:

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: frontend-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: frontend
  template:
    metadata:
      labels:
        app: frontend
    spec:
      containers:
      - name: nginx
        image: nginx
```
## Key Takeaways
    1. Static Pods: Managed directly by the kubelet, useful for system components.
    2. Manual Scheduling: Bypass the scheduler by explicitly assigning Pods to nodes.
    3. Labels: Key-value pairs for organizing and selecting objects.
    4. Selectors: Used to filter and select objects based on labels.