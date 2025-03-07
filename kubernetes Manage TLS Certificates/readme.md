# generate Certifications

## command to generate keys
```sh
openssl genrsa -out newadmin.key 2048
openssl req -new -key newadmin.key -out newadmin.csr -subj "/CN=newadmin"
```
## To create csr yaml

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

## to convert base64 csr certificate
```sh
cat newadmin.csr | base64 | tr -d "\n"
```

## Then copy the certificate and replace with above

## Then apply
```sh
k apply -f csr.yaml
```

## Then check the status 
```sh
k get csr
k describe csr newadmin
```
## To approve the certificate
```sh
k certificate approve newadmin
```
## Then generate the issued certificate yaml
```sh
k get csr newadmin -o yaml > issuesedcert.yaml
```
## Then again decode it with base64, then share to add it in kubeconfig