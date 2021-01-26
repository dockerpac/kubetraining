```
# Have a private repository in a registry

# Creates a deployment with Imagepullpolicy = always (defaut : notpresent)
kubectl apply -f deploy.yaml
# See that pods are not running
kubectl get pods
kubectl describe pod 

# cleanup
kubectl delete -f deploy.yaml


# Create a secret that will hold our private registry credentials
kubectl create secret docker-registry regcred --docker-server=https://UCP_URL --docker-username=admin --docker-password=xxxx --docker-email=me@me.com

kubectl get secrets
kubectl get secret regcred -o yaml

# Create a Deployment that will use our registry credentials
kubectl apply -f deploy_with_creds.yaml

# CLEANUP
kubectl delete -f deploy_with_creds.yaml

# Patch default service account
kubectl get serviceaccount
kubectl describe serviceaccount default

kubectl patch serviceaccount default -p '{"imagePullSecrets": [{"name": "regcred"}]}'

# or possible to create another ServiceAccount, and use it to run Pods

# CLEANUP
kubectl delete secrets/regcred
kubectl delete -f deploy.yaml
kubectl delete sa default
```
