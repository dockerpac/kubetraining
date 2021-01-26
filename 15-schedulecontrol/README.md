```
# Creates a Deployment with a scheduling control
# See that pods stay in Pending state
kubectl apply -f deploy_selectnode.yaml

kubectl get pods
kubectl describe pod demo-deployment-
# No node is matching the node selector


# Label a node with the selector that is wanted by the deployment
kubectl label node minikube gpu=true
# Now pods are running
kubectl get pods

# Show the label
ubectl get node minikube -o json | jq .metadata.labels

# Add a taint on the node, to tell it to evict every pod that doesn't tolerate mytoleration=value:NoExecute
# be careful, all the pods running on the node will be evicted
kubectl taint node mytoleration=value:NoExecute

kubectl get node minikube -o json | jq .spec.taints

# Pods are evicted and must be scheduled on another node
# But since we have a node selector in the deployment, the pods stay in Pending state

# Update the deployment to add the tolerations to the taint
kubectl apply -f deploy_toleration.yaml
# Now pods are scheduled on the node

# CLEANUP
kubectl delete -f deploy_toleration.yaml

# Remove label on node and taint
kubectl label node minikube gpu-
kubectl taint node minikube mytoleration-

```
