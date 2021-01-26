```
# Create pod with Volume hostpath
minikube ssh
cd /data/
rm -rf podhostpath

kubectl apply -f pod_hostpath.yaml
kubectl exec -it pod-using-hostpath -- sh -c 'echo "Hello World!" > /alpine-data/helloworld.txt'
ls -l /data
# Not recommended because different on each node (except when doing nfs mount on hosts)
# Need admin privilege to be able to use hostpath, because of security concerns

# NFS example
kubectl apply -f nfs_server.yaml
kubectl -n nfs get all

# Put DNS name of service in pod_nfs.yaml
kubectl apply -f pod_nfs.yaml
# Pod still pending
kubectl describe pod pod-using-nfs
# Show that url is not working on minikube host



# Change pod to use service ip
kubectl -n nfs get svc
kubectl edit pod pod-using-nfs
# Show that not all fields can be changed
kubectl delete pod pod-using-nfs
# edit file and apply
kubectl apply -f pod_nfs.yaml
# show files on minikube

# CLEANUP
kubectl delete -f pod_hostpath.yaml -f pod_nfs.yaml


# PERSISTENTVOLUMES
# Create 3 5Gi PV
kubectl apply -f pv_hostpath.yaml
kubectl get pv
kubectl describe pv pv-1


# Create 1 pvc
kubectl apply -f pvc_hostpath-1.yaml
kubectl get pvc
kubectl get pv
# PVC are namespaced, PV are cluster scoped

# Create a pod using this pvc
kubectl apply -f pod_pvc-1.yaml
kubectl exec -it pod-pvc-1 -- sh
echo "hello" > /alpine-data/hello.txt

# Delete pod and pvc
kubectl delete -f pod_pvc-1.yaml -f pvc_hostpath-1.yaml
# See that Status=Released, but still associated to a claim



# Reclaim Policy = Retain -->  Means PV is not deleted automatically
# Reclaim Policy = Delete --> Means PV is deleted

# CLEANUP
kubectl delete -f pv_hostpath.yaml

# Dynamic provisionning
# Storage class hostpath



# Storage class
kubectl apply -f storageclass_nfs.yaml
kubectl get sc

# Create PVC with storage class
kubectl apply -f pvc_storageclass.yaml

kubectl get pvc
kubectl get pv

# Delete pvc and see that pv is automatically deleted
kubectl delete pvc pvc-1
kubectl get pvc
kubectl get pv


# STORAGE CLASS NFS
# Install nfs provisionner
cd nfs-provisionner
# edit deployment.yaml and put IP service NFS
kubectl apply -f rbac.yaml -f deployment.yaml -f class.yaml
kubectl get pods

# Create pvc
kubectl apply -f test-claim.yaml
kubectl get pvc
kubectl get pv

# Show files on minikube host
ls /data/nfs

# Delete pvc
kubectl delete pvc test-claim
kubectl get pv

# Create another pvc, and see that a new pv has been created
kubectl apply -f test-claim.yaml
kubectl get pvc
kubectl get pv

# CLEANUP
kubectl delete -f .
kubectl delete PV_LIST
kubectl delete -f storageclass_hostpath.yaml -f nfs_server.yaml

```
