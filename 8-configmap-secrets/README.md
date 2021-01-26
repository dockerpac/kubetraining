```
# List files
ls -l configure-pod-container/configmap/
# Create the configmap
kubectl create configmap game-config --from-file=configure-pod-container/configmap/

kubectl get configmaps
kubectl describe configmap game-config

# Create configmap from one file
kubectl create configmap game-config-2 --from-file=mygame.prop=configure-pod-container/configmap/game.properties
kubectl get configmaps
kubectl describe configmap game-config-2

# Create configmap with env file
kubectl create configmap game-config-env-file --from-env-file=configure-pod-container/configmap/game-env-file.properties
kubectl get configmaps
kubectl describe configmap game-config-env-file

# Consume in Pods
# env file
kubectl apply -f pod_env.yaml
kubectl exec -it pod-cm-env -- env

# mount volume
kubectl apply -f pod_mount.yaml
kubectl exec -it pod-cm-mount -- sh
ls -l /etc/config
cat /etc/config/ui.properties




# SECRETS
# Show that if secret not exists, then pod fails
kubectl apply -f pod_secret.yaml
kubectl get pods
kubectl describe pod pod-secret

# Create secret from file
kubectl create secret generic db-user-pass --from-file=username=./username.txt --from-file=password=./password.txt

kubectl get secrets
kubectl describe secret db-user-pass

kubectl get secret db-user-pass -o yaml
echo "bXktc2VjdXJlLXBhc3N3b3JkCg==" | base64 --decode

# Get secrets into Pod
kubectl apply -f pod_secret.yaml
kubectl exec -it pod-secret -- sh
env

# CLEANUP
kubectl delete -f pod_env.yaml -f pod_mount.yaml -f pod-secret.yaml
kubectl delete configmap/game-config configmap/game-config-2 configmap/game-config-env-file secret/db-user-pass
```