# KUBERNETES101

All this examples were done using [minikube](https://github.com/kubernetes/minikube)


## TIPS AND TRICKS
When using Minikube, instead of pushing your Docker image to a registry, you can simply build the image using the same Docker host as the Minikube VM, so that the images are automatically present. To do so, make sure you are using the Minikube Docker daemon:
- eval $(minikube docker-env)
- eval $(minikube docker-env -u) # when you no longer wish to use the Minikube host, you can undo this change.

- `export CLUSTER_IP=$(kubectl get services/<SERVICE_NAME> -o go-template='{{(index .spec.clusterIP)}}')
   echo CLUSTER_IP=$CLUSTER_IP
   curl $CLUSTER_IP:80`                                        # After deploying, the service can be accessed via the ClusterIP allocated

## COMMANDS
```bash
kubectl cluster-info
kubectl config get-contexts
kubectl config use <CONTEXT-NAME>
kubectl get services                                    # List all services in the namespace
kubectl describe services <SERVICE_NAME>
kubectl get deployment my-dep                           # List a particular deployment
kubectl get nodes -o yaml
kubectl get rc                                          # Get the replication controller
kubectl get cs                                          # Get the component statuses
kubectl get deployments
kubectl get events                                      # Get cluster events
kubectl describe service <SERVICE_NAME> | grep NodePort # Get the assigned NodePort using kubectl.
kubectl describe svc/<SERVICE_NAME>
kubectl config view                                     # view kubectl config
kubectl create ns <NAMESPACE_NAME>                      # Create a namespace
kubectl get deployment/<DEEPLOYMENT_NAME> -o yaml       # Get deployment yaml
kubectl -n kube-system describe secret $(kubectl -n kube-system get secret | grep admin-user | awk '{print $1}') # Get user token
```

### PODs
```bash
kubectl get pods --all-namespaces                                                    # List all pods in all namespaces
kubectl get pods -o wide                                                             # List all pods in the namespace, with more details
kubectl get pods -o wide --all-namespaces                                            # List all pods in all namespaces, with more details
kubectl get pods --include-uninitialized                                             # List all pods in the namespace, including uninitialized ones
kubectl get pods --sort-by=.metadata.name                                            # sorts pods by name
kubectl get pods -o wide --all-namespaces                                            # returns more details
kubectl get pods/<POD_NAME> -n <NAMESPACE_NAME> -o json                              # returns the pod json
kubectl get pods  -n <NAMESPACE_NAME> -o=jsonpath="{..image}" -l app=cart-dev        # searches cart-dev, and returns the image based on the jsonpath
kubectl get pods --all-namespaces -o jsonpath="{.items[*].spec.containers[*].image}" # all container images running
```

### Show information about one specific namespace (ex: kube-system)
```bash
kubectl -n kube-system get pods
kubectl -n kube-system get nodes
kubectl -n kube-system get services
kubectl -n kube-system edit service kubernetes-dashboard
kubectl -n kube-system get nodes -o yaml
kubectl -n kube-system get services kube-dns
```

## TOOLS

### CALICO
```bash
kubectl apply -f http://docs.projectcalico.org/v2.1/getting-started/kubernetes/installation/hosted/kubeadm/1.6/calico.yaml # Install
kubectl annotate ns <NAMESPACE_NAME> "net.beta.kubernetes.io/network-policy={\"ingress\":{\"isolation\":\"DefaultDeny\"}}" # Annotate the <NAMESPACE_NAME> namespace to deny all incoming (ingress) traffic. Now, remote access to the pods inside the <NAMESPACE_NAME> should be unavailable, and you should receive a timeout warning.
```

### HELM
```bash
helm install stable/grafana --name grafana -n <NAMESPACE_NAME>
helm install stable/prometheus --name prometheus -n <NAMESPACE_NAME>
```
