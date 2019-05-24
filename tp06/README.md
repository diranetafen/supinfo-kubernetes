# this guide resume how to use src file

## ** kubectl alias **
we set an alias for the exercise purpose

```
alias k="kubectl -n=imageresizer"
```
## ** create headless service for minio app **

```
kubectl create -f simple-service-minio-headless.yml -n=imageresizer
k get svc
```

## ** create statefull minio app **

```
kubectl create -f simple-minio-statefulset.yml -n=imageresizer
k get statefulset
k get pods
```

## ** create cluster service for minio app **
We need to expose our minio cluster through cluster IP

```
kubectl create -f simple-service-minio-clusterip.yml -n=imageresizer
k get svc
```