```
# Create namespace for dev team and tiller serviceaccount
kubectl create namespace app1-dev
kubectl create serviceaccount tiller --namespace app1-dev

# Configure helm with only this serviceaccount and this namespace
helm init --service-account tiller --tiller-namespace app1-dev


# No TLS by default

# Install chart
helm install stable/phpmyadmin --tiller-namespace app1-dev
Error: release understood-jackal failed: namespaces "default" is forbidden: User "system:serviceaccount:app1-dev:tiller" cannot get namespaces in the namespace "default": access denied

# Add grants to service account and test again installation
kubectl -f .
```