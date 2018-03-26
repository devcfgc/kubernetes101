# Create Ingress Routing

## Goals
1. Create Deployment
2. Deploy Ingress
3. Deploy Ingress Rules
4. Test

### Create Deployment
```
kubectl apply -f deployment.yaml
kubectl get deployment
```

### Deploy Ingress
```
kubectl create -f ingress.yaml
kubectl get rc
```

### Deploy Ingress Rules
The rules apply to requests for the host my.kubernetes.example. Two rules are defined based on the path request with a single catch all definition. Requests to the path /webapp1 are forwarded onto the service webapp1-svc. Likewise, the requests to /webapp2 are forwarded to webapp2-svc. If no rules apply, webapp3-svc will be used.

```
kubectl create -f ingress-rules.yaml
kubectl get ing
```

### Test

With the Ingress rules applied, the traffic will be routed to the defined place. The first request will be processed by the webapp1 deployment.

```
curl -H "Host: my.kubernetes.example" 172.17.0.108/webapp1
```
```
curl -H "Host: my.kubernetes.example" 172.17.0.108/webapp2
```
```
curl -H "Host: my.kubernetes.example" 172.17.0.108
```
