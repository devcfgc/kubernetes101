# Basic tutorial

## First we need to install Calico
1) kubectl apply -f http://docs.projectcalico.org/v2.1/getting-started/kubernetes/installation/hosted/kubeadm/1.6/calico.yaml
2) kubectl create ns policy-demo
## Create some nginx pods in the policy-demo namespace
3) kubectl run --namespace=policy-demo nginx --replicas=2 --image=nginx
## Expose the pods trhough a service
4) kubectl expose --namespace=policy-demo deployment nginx --port=80
5) kubectl create -f pod.yaml
6) kubectl exec -n policy-demo client -- wget -T 2 -q nginx  -O -
## Enable isolation: annotate the policy-demo namespace to deny all incoming (ingress) traffic
7) kubectl annotate ns policy-demo "net.beta.kubernetes.io/network-policy={\"ingress\":{\"isolation\":\"DefaultDeny\"}}"
## We should get a timeout
8) kubectl exec -n policy-demo client -- wget -q nginx  -T 2 -O -
## Allow access with the network-policy.yaml
9) kubectl create -f network-policy.yaml
## Everything 200
10) kubectl exec -n policy-demo client -- wget -q nginx  -T 2 -O -
