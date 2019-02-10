# this guide resume how to use src file

## ** create local disk 500Go for postgresql data **
To store data permanently, we need `persistent volume`
Because we are working with local cluster, we will create local disk dedicated at this task inspired by [serverascode](https://serverascode.com/2018/09/19/persistent-local-volumes-kubernetes.html) tutorial

```
mkfs.ext4 /dev/sdb
mkdir -p /mnt/disks/sdb
mount /dev/sdb /mnt/disks/sdb
lsblk
```

## ** kubectl alias **
we set an alias for the exercise purpose

```
alias k="kubectl -n=imageresizer"
```

## ** create namespace **
```
kubectl create -f namespace-imageresizer.yml
```

## ** create persistent volume **
```
kubectl create -f storage-class.yml -n=imageresizer
k get sc
kubectl create -f local-pv-sdb.yml -n=imageresizer
k get pv
kubectl create -f local-pvc-sdb.yml -n=imageresizer
k get pvc
```

## ** create postgres configmap **
```
kubectl create configmap postgres-tables --from-file=tables.sql -n=imageresizer
k get configmap 
```

## ** deploy postgres **
```
kubectl create -f simple-postgres-deployment.yml -n=imageresizer
k get deployment
k get pods
kubectl create -f simple-service-postgres-clusterip.yml -n=imageresizer
k get service
```

## ** deploy image-api **
```
kubectl create -f simple-image-api-deployment.yml -n=imageresizer
k get deployment
k get pods
k logs -f pod/image-api-deployment-58ffff6658-xcvt2
kubectl create -f simple-service-image-api-clusterip.yml -n=imageresizer
k get service
kubectl create -f image-api-ingress.yml -n=imageresizer
```

Before verify that is working as expected, we need to edit our host file
```
echo "127.0.0.1 image-api.k8s.lan" | sudo tee -a /etc/hosts
```
