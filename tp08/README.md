# this guide resume how to use src file

## ** kubectl alias **
we set an alias for the exercise purpose

```
alias k="kubectl -n=monitoring"
```

## * Deploy Kubernetes-Prometheus * ##

```
kubectl create namespace monitoring

helm repo add coreos https://s3-eu-west-1.amazonaws.com/coreos-charts/stable
helm repo up 

helm install coreos/prometheus-operator --name prometheus-operator --namespace monitoring

helm install coreos/kube-prometheus --name kube-prometheus --namespace monitoring
kubectl port-forward -n monitoring $(kubectl get pod -n monitoring -l app=kube-prometheus-grafana -o jsonpath='{.items[0].metadata.name}') 3000:3000 --address 0.0.0.0
```

## * Deploy EFK * ##

```
minikube addons enable efk
kubectl port-forward -n kube-system $(kubectl get pod -n kube-system -l k8s-app=kibana-logging -o jsonpath='{.items[0].metadata.name}') 5601:5601 --address 0.0.0.0
```

## * Deploy Heptio Valero * ##

```

```