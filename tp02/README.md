# this guide resume how to use src file

## ** Installation cluster **
The installation of the cluster is inspired by this tutorial on [digitalocean](https://www.digitalocean.com/community/tutorials/how-to-create-a-kubernetes-1-10-cluster-using-kubeadm-on-centos-7)

We use [ansible](https://www.ansible.com/) to make it very easily

```
ansible-playbook -i hosts ~/kube-cluster/kube-dependencies.yml
ansible-playbook -i hosts ~/kube-cluster/master.yml
kubectl get nodes
ansible-playbook -i hosts ~/kube-cluster/workers.yml

#wait 35 seconds (depending of the size of your VM)
kubectl get nodes
```

## ** Install Dashboard **
The installation of the cluster is inspired by this tutorial on [dzone](https://dzone.com/articles/how-to-install-the-kubernetes-dashboard)

```
kubectl create -f https://raw.githubusercontent.com/kubernetes/dashboard/master/aio/deploy/recommended/kubernetes-dashboard.yaml
kubectl proxy
curl http://localhost:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/
kubectl create -f kubernetes-dashboard-user.yml
kubectl create -f kubernetes-dashboard-role.yml
```
To retrieve the secret that will be use to connect on kubernetes Dashboard

```
kubectl -n kube-system describe secret $(kubectl -n kube-system get secret | grep admin-user | awk '{print $1}')`
```
