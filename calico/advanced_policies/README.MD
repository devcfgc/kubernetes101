# Advanced tutorial

## First we need to install Calico
1) kubectl apply -f http://docs.projectcalico.org/v2.1/getting-started/kubernetes/installation/hosted/kubeadm/1.6/calico.yaml
## Create Namespace and Enable Isolation
2) kubectl create ns advanced-policy-demo
3) kubectl annotate ns advanced-policy-demo "net.beta.kubernetes.io/network-policy={\"ingress\":{\"isolation\":\"DefaultDeny\"}}"
## Setup and Expose Services
4) kubectl run --namespace=advanced-policy-demo nginx --replicas=2 --image=nginx
5) kubectl expose --namespace=advanced-policy-demo deployment nginx --port=80
## Test Connection
6) kubectl run --namespace=advanced-policy-demo access --rm -ti --image busybox /bin/sh
### You should have received a timeout warning (because the pods in this namespace are isolated).
7) wget -q --timeout=5 nginx -O -
### Exit from the pod
8) exit
## Install and Configure calicoctl
9) curl -O -L https://github.com/projectcalico/calicoctl/releases/download/v1.3.0/calicoctl
### Ensure that the downloaded calicoctl file is executable
10) chmod +x calicoctl
11) mv calicoctl /bin/calicoctl
### For this tutorial, we will be using etcd version 2 (bundled with our Calico install). To set these configurations, simply set DATASTORE_TYPE and ETCD_ENDPOINTS using the export command.
12) export DATASTORE_TYPE=etcdv2
    export ETCD_ENDPOINTS=http://localhost:6666
13) calicoctl get nodes
## Review Namespace Profile
### You should see advanced-policy-demo in the profile
14) calicoctl get profile -o wide
### You can also use calicoctl to review the advanced-policy-demo YAML
15) calicoctl get profile k8s_ns.advanced-policy-demo -o yaml
## Define an Ingress Policy
### let's create a NetworkPolicy which allows traffic to nginx pods from any pods in the advanced-policy-demo Namespace
16) kubectl create -f nginx-network-policy.yaml
### To verify the policy was appropriately applied, you can use calicoctl to get the policy, with its order and selectors
17) calicoctl get policy -o wide
## Test Internal Connection
18) kubectl run --namespace=advanced-policy-demo access --rm -ti --image busybox /bin/sh
### If successful, you should see the NGINX default page returned (in HTML format).
19) wget -q --timeout=5 nginx -O -
20) exit
## Prevent Outgoing Connections from Pods
### The next policy we will create will prevent all outgoing traffic from pods within the advanced-policy-demo namespace.
21) calicoctl apply -f calico-policy-deny-egress.yaml
### Now let's create another policy that allows traffic to kube-dns. You'll notice that the order of this policy is 400, whereas the order for the above policy is 500 - this is so that this new policy takes priority over the policy blocking ALL outgoing traffic. You'll also notice this policy only allows outgoing traffic for a specific protocol and to a specific destination.
22) calicoctl apply -f calico-policy-allow-dns.yaml
## Test Connections
### we'll try making an outgoing call to another pod within the advanced-policy-demo namespace. To do this, create another pod
23) kubectl run --namespace=advanced-policy-demo access --rm -ti --image busybox /bin/sh
### Once again, you should get the default NGINX HTML.
24) wget -q --timeout=5 nginx -O -
### Testing External Connections
25) ping google.com
#### You'll notice the pod is unable to ping any outside services as they've been blocked by our policy.

> If we wanted to enable specific egress traffic, we could create another policy similiar to the kube-dns policy to allow traffic to only that specific destination (and on specific protocols or ports).
