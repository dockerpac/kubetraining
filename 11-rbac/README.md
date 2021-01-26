
```
# Creating a Namespace
kubectl apply -f namespace.yaml

kubectl get ns

# Create a pod
kubectl apply -f pod_nginx.yaml

# Search for the pod
kubectl get pods | grep nginx-ns
kubectl get pods --all-namespaces | grep nginx-ns

# Recreate the Pod in the test namespace
kubectl delete -f pod_nginx.yaml
kubectl -n test apply -f pod_nginx.yaml

kubectl get pods --all-namespaces | grep nginx-ns
kubectl get pods | grep nginx-ns
kubectl -n test get pods

# In another terminal, source the bundle of a user that has no permission
# See that the user has no permissions
kubectl get pods
kubectl -n test get pods

# Create a ClusterRole pod-reader
kubectl apply -f clusterrole_view.yaml

kubectl get clusterroles
kubectl describe clusterrole pod-reader

# Create a RoleBindig
# edit the file with the name of the user
kubectl apply -f rolebinding_view.yaml

kubectl get rolebindings
kubectl -n test get rolebindings
kubectl -n test describe rolebindings read-pods

# Try again to list 
kubectl get pods
kubectl -n test get pods


# CLEANUP
kubectl -n test delete -f rolebinding_view.yaml -f pod_nginx.yaml -f clusterrole_view.yaml -f namespace.yaml

unset KUBECONFIG
```