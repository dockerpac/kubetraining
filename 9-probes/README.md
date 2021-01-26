```
# Create liveness test
# Show that Pods are restarting
kubectl apply -f liveness.yaml

kubectl get pods
kubectl get events
kubectl describe pod liveness-deployment-

# CLEANUP
kubectl delete -f liveness.yaml

# Create readiness test
kubectl apply -f readiness.yaml
# Show that Pods IP are populated in Service endpoints
kubectl describe svc destination-entrypoint
# Simulate an error of Readiness in on Pod
kubectl exec -it readiness-deployment-7d8df88986-8dzkz -- rm /tmp/healthy
# Show that failing Pod is removed from endpoints
kubectl describe svc destination-entrypoint

# CLEANUP
kubectl delete -f readiness.yaml
```


