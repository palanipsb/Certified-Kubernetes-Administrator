## To create namespace imperative approach

```linux
kubectl create ns "dev"
```

## to get ns

```linux
kubectl get namespaces
```

# Set the default Namespace for your current context:

```
kubectl config set-context --current --namespace=my-namespace

```

# to create deploy

```
kubectl create deploy nginx-dev --image=nginx -n dev

kubectl get deploy -n dev

k get pods -o wide

k exec -it nginx-test-b548755db-6f2sl -- sh

k scale --replicas=3 deploy/nginx-test

k expose deploy/nginx-dev --name svc-dev --port 80 -n dev

k get svc -n dev

k get svc -n dev -w

k exec -it nginx-dev-6bcfddddd5-s9skf -n dev -- sh

curl svc-test

curl: (6) Could not resolve host: svc-test

cat /etc/resolv.conf #to check fully qualified name

curl svc-test.default.svc.cluster.local # then we can connect with this to another pod

```
