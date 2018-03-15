# Calicoctl

## First we need to install Calico
1) kubectl apply -f http://docs.projectcalico.org/v2.1/getting-started/kubernetes/installation/hosted/kubeadm/1.6/calico.yaml
## Install calicoctl
2) curl -O -L https://github.com/projectcalico/calicoctl/releases/download/v1.3.0/calicoctl
3) chmod +x calicoctl
4) mv calicoctl /bin/calicoctl
## Configure to use etcd
### For this tutorial, we will be using etcd version 2 (bundled with our Calico install). To set these configurations, simply set DATASTORE_TYPE and ETCD_ENDPOINTS using the export command.
5) export DATASTORE_TYPE=etcdv2
   export ETCD_ENDPOINTS=http://localhost:6666
> OR Configure to use KDD - We can also configure calicoctl to use the Kubernetes Datastore Driver instead **For advanced usage, etcd is recommended** To do this, we would simply set the DATASTORE_TYPE to kubernetes and set KUBECONFIG, like so:
```
export DATASTORE_TYPE=kubernetes
export KUBECONFIG=~/.kube/config
```
## Testing Configuration
6) calicoctl get nodes

