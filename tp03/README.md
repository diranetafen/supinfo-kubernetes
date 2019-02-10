# this guide resume how to use src file

## ** pod  **
To create the nginx pod
```
kubectl create -f simple-pod-nginx.yml
kubectl describe pods nginx
```

To make the pods reachable from `outside`

```
kubectl port-forward nginx 8888:80
```

To delete the pods
```
kubectl delete -f simple-pod-nginx.yml
```

## ** configmap **

To create nginx's configmap that enable customization for nginx
```
kubectl create configmap nginx-conf --from-file=nginx-conf/
```

Finally we starts nginx's pod with configmap
```
kubectl create -f simple-pod-nginx-with-config-map.yml
```

## ** deployment **
In this part we will `deploy`, `replicate`, and `rollout` to manage nginx's versionning using `kubernetes deployment`
```
kubectl create -f simple-deployment-nginx.yml
kubectl get pods -o wide
kubectl get deployment -o wide
kubectl get replicasets --selector=app=nginx -o wide
kubectl apply -f simple-deployment-nginx-update.yml
kubectl get pods -o wide -w
kubectl rollout history deployment nginx-deployment
kubectl rollout status deployment nginx-deployment
kubectl get replicasets --selector=app=nginx -o wide
kubectl rollout undo deployment nginx-deployment
kubectl get replicasets --selector=app=nginx -o wide
```

## ** namespace **
Now we will create namespace to `isolate` our deployment
```
kubectl create -f namespace.yml
kubectl create -f simple-deployment-nginx-update.yml -n production
kubectl get deployment -o wide -n production
```
