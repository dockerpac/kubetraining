```
# First, get informations on node
kubectl describe node minikube
kubectl get node minikube -o json | jq .status.allocatable

# Test Request on a Pod
kubectl apply -f deploy_request.yaml

# Some Pods are Pending
kubectl get pods
# Describe pending pod and see that there is not enough memory
kubectl describe pod 

# Show again the available resources on the node
kubectl describe node minikube

# CLEANUP
kubectl delete -f deploy_request.yaml


## Limit
kubectl apply -f pod_outofbound.yaml

# Show OOM in action
kubect get pods --watch


# Get instant metrics using metrics-server
kubectl apply -f pod_stress.yaml

kubectl top node
kubectl top pods


# CLEANUP
kubectl delete -f pod_outofbound.yaml -f pod_stress.yaml
```