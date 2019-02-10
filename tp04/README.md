# this guide resume how to use src file

## ** labels and selectors **

We will create `labels and selectors` to make smart search
```
kubectl delete -f simple-deployment-nginx-update.yml --namespace=production
kubectl create -f simple-deployment-nginx-update-selector.yml --namespace=production
kubectl get po --show-labels
kubectl get pods  -l app=nginx --namespace=production
kubectl get pods  -l env!=production --namespace=production
kubectl get pods  -l 'env in (production)' --namespace=production
kubectl get pods  -l 'app notin (nginx)' --namespace=production
```

## ** services **

It's time to access our web-server nginx using `kunernetes services`
To understand how it's work you can find explanation in [kubernetes](https://kubernetes.io/docs/tutorials/services/) and [stackaverflow](https://stackoverflow.com/questions/43789155/nodeport-service-is-not-externally-accessible-via-port-number)

```
kubectl create -f simple-service-nginx-nodeport.yml --namespace=production
kubectl get service -o wide --all-namespaces
kubectl describe svc service-nginx-nodeport --namespace=production
```

How to access the [cluster/service](https://kubernetes.io/docs/tutorials/services/#source-ip-for-services-with-type-clusterip) ip ?
```
kubectl create -f simple-service-nginx-clusterip.yml --namespace=production
kubectl get service -o wide --all-namespaces
kubectl describe svc service-nginx-clusterip --namespace=production
```

## ** ingress **
This part is inspired by [akomljen](https://akomljen.com/kubernetes-nginx-ingress-controller/) tutorial

```
kubectl create -f ingress-controller.yml
kubectl get pods -n=ingress-nginx
kubectl create -f app-ingress.yml -n=production
kubectl get service --all-namespaces
```

How to verify ?
```
echo "127.0.0.1 website.k8s.lan" | sudo tee -a /etc/hosts
curl http://website.k8s.lan:30080
```

We clean our work
```
kubectl delete -f ingress-controller.yml
kubectl delete -f app-ingress.yml -n=production
```

## ** service discovery **
It's time to see all our deployed services throught `service discovery`
```
kubectl create -f simple-deployment-kuard.yml -n=production
kubectl get pods -n=production
kubectl port-forward kuard-deployment-69bb886c4-64lwg 8080:8080 -n=production
```
