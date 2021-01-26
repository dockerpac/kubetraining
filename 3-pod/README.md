```
# imperative commands
# create pod
kubectl run nginx --image nginx:latest --generator=run-pod/v1

kubectl get pods
kubectl get pods nginx -o wide --show-labels

# edit using command line, replace image with broken one nginx:foobar
kubectl edit pod nginx
# change image broken
# see that pod fails to pull image
kubectl get pod nginx
kubectl describe pod nginx
kubectl get events

# export current object to yaml
kubectl get pods nginx -o yaml > pod_nginx_broken.yaml
# Change image to nginx:latest in the file and use apply
kubectl apply -f pod_nginx_broken.yaml

# Delete pod
kubectl delete pod nginx

# Create multi container pod
kubectl apply -f pod_nginx_alpine.yaml
# See 2/2 READY
kubectl get pods

# exec in container
# exec by default in 1st container
kubectl exec -it nginx -- sh

# show all the containers
kubectl describe pod/nginx -n default

# exec in alpine container
kubectl exec -it nginx -c alpine -- sh

# curl to nginx container
apk add curl
curl http://localhost

# add a file to /alpine-data
echo "hello world" > /alpine-data/hello.txt

# Open another terminal to watch pods
kubectl get pods --watch

# exec in nginx container
kubectl exec -it nginx -c nginx -- sh
ls -l /nginx-data

# kill container
kill 1

# show that container is restarted
kubectl describe pod nginx

# Export port to local workstation
kubectl port-forward nginx 8080:80

# view logs
kubectl logs nginx -c nginx


# delete object
kubectl delete pod nginx

# no scale, no rollback, if node die, the pod die
# need a way to control pods --> controllers
```