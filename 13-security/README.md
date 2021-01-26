```
# Create a pod without a securityContext
kubectl apply -f priv-escalation.yaml

kubectl exec -it priv-escalation -- sh
id
sudo su -
# OK

# Create a pod with a securityContext
kubectl apply -f priv-escalation-deny.yaml

kubectl exec -it priv-escalation-deny -- sh
id
sudo su -
# KO !

# CLEANUP
kubectl delete -f priv-escalation.yaml -f priv-escalation-deny.yaml

# ResourceQuota
# Apply quotas on namespace default
kubectl apply -f resourcequota.yaml
kubectl get resourcequotas
kubectl describe quota compute-quota
kubectl describe quota object-quota

kubectl describe namespace default

# Create a Deployment without Limits and Requests
# See that it fails
kubectl apply -f deploy_noquota.yaml
kubectl get all

# To see the reason, we have to get the events of the namespace
kubectl get events


# LimitRange
kubectl apply -f limitrange.yaml
kubectl describe namespace default

# Now the deployment is OK
# but only 2 replicas are up

# CLEANUP
kubectl delete -f deploy_noquota.yaml -f resourcequota.yaml -f limitrange.yaml

```