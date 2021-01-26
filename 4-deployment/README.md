```
# Deploy 
kubectl apply -f deploy.yaml

# Get all
kubectl get all


# Show RS ID, PODS ID

# Get
kubectl get deploy,pods

# Show labels generated
kubectl get pods --show-labels
# Show nodes where pods are deployed
kubectl get pods -o wide

# In another terminal, watch for pods
kubectl get pods -l app=demo-pods --watch
# Delete a pod
kubectl delete pod demo-deployment-

# Describe RS
kubectl describe rs demo-deployment-

# Describe Deployment
kubectl describe deployment demo-deployment

# Change number of replicas
# See that 2 pods are created, then terminated
kubectl scale --replicas=5 replicaset demo-deployment-

# Scale deployment using imperative command
kubectl scale --replicas=5 deploy/demo-deployment


# Do a rolling update
# change image in the deployment
kubectl edit deployment demo-deployment

# See rollout status
kubectl rollout status deploy/demo-deployment

# Do a rolling update of failed image
kubectl rollout undo deploy/demo-deployment

# use the yaml file to delete objects
kubectl delete -f deploy.yaml

```