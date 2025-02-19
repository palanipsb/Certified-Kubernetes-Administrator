#Kubernetes Commands
Aliases for Common Commands:
alias k="kubectl"
alias kgd="k get deploy"
alias kgp="k get pods"
alias kgn="k get nodes"
alias kgs="k get svc"
alias kge="k get events --sort-by='.metadata.creationTimestamp' | tail -8"

Aliases for Describing Kubernetes Resources:
alias kdp="kubectl describe pod"
alias kdd="kubectl describe deployment"
alias kds="kubectl describe service"
alias kdn="kubectl describe node"

Exporting Variables:
export nks="-n kube-system"
kubectl get pv -n kube-system — sort-by=.spec.capacity.storage
kubectl get pv -n kube-system

#After cluster creation
kubectl cluster-info --context kind-cka-cluster2

#To see if node is running
kubectl get nodes

#To Create pods dry run
#Dry run, but not created
kubectl run nginx --image=nginx --dry-run=client

#to get output in yaml file format from imperative approach
kubectl run nginx --image=nginx --dry-run=client -o yaml

#to copy the yaml content to new file
kubectl run nginx --image=nginx --dry-run=client -o yaml > pod-new.yaml

#To Create pods
kubectl run ngonx-pod --image=nginx:latest
#After creating pods, below commands to manage pods
kubectl get pods
kubectl explain pod
kubectl describe pod ngonx-pod
kubectl edit pod nginx-pod
kubectl exec -it nginx-pod -- sh
kubectl get pods nginx-pod --show-labels
kubectl get pods -o wide
