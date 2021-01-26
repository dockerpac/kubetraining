```
# Make sure in UCP Scheduler settings that Admins are not authorized to deploy on managers
# Deploy DaemonSet
kubectl apply -f daemonset.yaml

# See that pods are only scheduled on 3 nodes / 5 nodes
# Use a label filter to list objects
kubectl get all -l k8s-app=fluentd-logging

# In UCP, change scheduler settings
# Delete and reapply daemonset
kubectl delete -f daemonset.yaml
kubectl apply -f daemonset.yaml

# Clean up
kubectl delete -f daemonset.yaml

unset KUBECONFIG
```