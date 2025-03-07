# generate Certifications
```sh
openssl genrsa -out newadmin.key 2048
openssl req -new -key newadmin.key -out newadmin.csr -subj "/CN=newadmin"
````
# The create csr yaml

```yaml
apiVersion: certificates.k8s.io/v1
kind: CertificateSigningRequest
metadata:
  name: newadmin
spec:
  request: <crs sertificate>
  signerName: kubernetes.io/kube-apiserver-client
  expirationSeconds: 86400  # one day
  usages:
  - client auth
  
```

# to convert base64 csr certificate
```sh
cat newadmin.csr | base64 | tr -d "\n"
```

# then copy the certificate and replace with above

# then apply
```sh
k apply -f csr.yaml
```

# then check the status 
```sh
k get csr
k describe csr newadmin
```
# To approve the certificate
```sh
k certificate approve newadmin
```
# then generate the issued certificate yaml
```sh
k get csr newadmin -o yaml > issuesedcert.yaml
```
## then again decode it with base64, then share to add it in kubeconfig

# Kubernetes authentication & autherization - RBAC

## To see access for ourself
```sh
k auth can-i get pod
k auth whoami
```
## To see access for newly created user
```sh
k auth can-i get pod --as newadmin
```
## create role.yaml
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: default
  name: pod-reader
rules:
- apiGroups: [""] # "" indicates the core API group, if like "apps", named group
  resources: ["pods"]
  verbs: ["get", "watch", "list"]
``` 
```sh
k apply -f role.yaml

k get roles

k describe roles pos-reader
```
## Create binding.yaml
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: read-pods
  namespace: default
subjects:
- kind: User
  name: newadmin
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: pod-reader
  apiGroup: rbac.authorization.k8s.io
```
```sh  
k get rolebinding
```
## now newadmin user will get access to pod
```sh
k auth can-i get pods --as newadmin
```
## To get all roles
```sh
k get roles -A
```
## to get count
```sh
k get roles -A --no-headers | wc -l
```
## to set current user as new user to check pods
```sh
k config set-credentials newadmin \
> --client-key=newadmin.key \
> --client-certificate=newadmin.csr
```
## To set new context for new user
```sh
k config set-context newadmin --cluster=kind-cka-custer3\
> --user=newadmin

k config get-contexts
```