```
# source bundle ucp-bundle-admin
# After sourcing the bundle, you have direct access to the cluster using your user credentials
kubectl version
kubectl cluster-info
kubectl get nodes
kubectl get pods

# source a test user and see that by default the user has no permission
# source bundle ucp-bundle-usertest

kubectl get nodes
kubectl get pods


# show kubectl internal config
kubectl config view
ENV | grep KUBE

unset KUBECONFIG
kubectl get nodes (minikube)

# Basics of kubectl
# get / describe / -o yaml / --show-labels / -o wide

kubectl api-resources
kubectl get nodes
kubectl get nodes -o wide --show-labels
kubectl describe node
kubectl get events
kubectl get node -o yaml
```