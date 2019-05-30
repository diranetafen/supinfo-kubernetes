# this guide resume how to use src file


## ** kubectl alias **
we set an alias for the exercise purpose

```
alias k="kubectl -n=imageresizer"
```

## * Install Helm * ##

```
cd
curl https://raw.githubusercontent.com/kubernetes/helm/master/scripts/get > get_helm.sh
chmod 700 get_helm.sh
./get_helm.sh
helm init
```

## * Deploy Nats * ##

```
helm install stable/nats --name nats  --namespace imageresizer --set auth.enabled=false
```

## * Deploy Worker * ##

```
helm install ./worker --name worker  --namespace imageresizer
```

## * Deploy Jobs-api * ##

```
helm install ./jobs-api --name jobs-api  --namespace imageresizer
```

## * Build  and deploy Frontend * ##

### Retrieve application source code ###

```
cd
git clone https://github.com/alexandrevilain/image-resizer.git
```

### update .env.prod ###

```
cd image-resizer/frontend
cat .env.prod
VUE_APP_JOB_API=http://jobs-api.imageresizer.svc.cluster.local:8000
VUE_APP_IMAGES_API=http://service-image-api-clusterip.imageresizer.svc.cluster.local:3000
```

### Build the container ###

```
docker build -t frontend .
```

### deploy the frontend ###

```
docker run -d --name frontend -p 8080:80 frontend
```
