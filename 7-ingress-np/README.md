```
# Deploy Nginx controller
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/nginx-0.22.0/deploy/mandatory.yaml

# Expose nginx ingress (for minikube)
kubectl expose deployment \
    -n ingress-nginx nginx-ingress-controller \
    --port 80 \
    --type LoadBalancer \
    --name ingress-nginx

# Create ingress rule
kubectl apply -f ingress.yaml

# Ingress is not shown when using get all
kubectl get all

kubectl get ingress

kubectl describe ingress my-app

# Get IP:PORT of the ingress service
nginx_service=$(minikube service ingress-nginx -n ingress-nginx --url)

# Not Found, because no Host HTTP Header provided
curl "$nginx_service"
# Hit correct service
curl "$nginx_service" -H "Host: my-app.com"

# CLEAN UP
kubectl delete svc/destination-entrypoint svc/destination-entrypoint-nodeport deploy/destination pod/alpine ingress/my-app


# NETWORKPOLICY
# doesn't work on minikube because flannel does not handle NP
# source bundle UCP
# goto 6-service
kubectl apply -f alpine.yaml -f clusterip.yaml -f deploy.yaml
kubectl get -f alpine.yaml -f clusterip.yaml -f deploy.yaml

# Connection is OK from another container
kubectl exec -it alpine -- ash
apk add curl
curl http://destination-entrypoint:8080

# In another terminal, watch traffic
kubectl exec -it alpine -- sh -c "while sleep 1 ; do curl http://destination-entrypoint:8080 ; done"
# goto 7-ingress-np
# Add a NetworkPolicy
kubectl apply -f networkpolicy_deny.yaml
kubectl exec -it alpine -- curl http://destination-entrypoint:8080
# Traffic is blocked !

kubectl get networkpolicies
kubectl desribe networkpolicy test-network-policy

# Apply a NetworkPolicy with to allow some ingress traffic
kubectl apply -f networkpolicy_allow.yaml
kubectl describe networkpolicy test-network-policy-allow
kubectl exec -it alpine -- curl http://destination-entrypoint:8080

# CLEANUP
kubectl delete svc/destination-entrypoint deploy/destination pod/alpine networkpolicy/test-network-policy networkpolicy/test-network-policy-allow

unset KUBECONFIG
```