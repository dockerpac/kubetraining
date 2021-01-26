```
# Create a test pod
kubectl apply -f alpine.yaml

# Create a deployment
kubectl apply -f deploy.yaml

# Show pods IP
kubectl get pods -o wide

# Test connectivity between pods using Pod IP
kubectl exec -it alpine -- ash
apk add curl
curl http://ip1:8000
curl http://ip2:8000

# Create Service ClusterIP
kubectl apply -f clusterip.yaml

# Get ClusterIP address and selector
kubectl get svc -o wide

# Test selector
kubectl get pods -l app=dest -o wide

# Show ClusterIP endpoints
# Same IPs as Pods
kubectl describe service destination-entrypoint
kubectl get endpoint 

# Test Service IP
kubectl exec -it alpine -- ash
curl http://ip:8080

# Show dns resolution configuration inside pod
curl http://destination-entrypoint:8080
cat /etc/resolv.conf

# Test from a node of the cluster
# Service IP is working but not DNS resolution
minikube ssh
curl http://ip:8080
curl http://destination-entrypoint:8080


# NODEPORT
kubectl apply -f nodeport.yaml

# Look for host random port
kubectl get svc
kubectl describe svc destination-entrypoint-nodeport

# Connect to Cluster nodeport
minikube ip
curl http://ip:port

# Try to change the NodePort to a fixed value
kubectl edit svc destination-entrypoint-nodeport

# Change port to 8080 -->  Not OK because not in NodePortRange
# Change port to 30500 --> OK

# NO CLEANUP
```