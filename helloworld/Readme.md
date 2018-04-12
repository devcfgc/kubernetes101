# Breaking down the helloworld app

## Goals
1. Create our deployment using YAML
2. Execute our deployment using YAML
3. Verify that the application is working as expected
4. Scale the helloworld application

### Create our deployment using YAML

If we were going to recreate our deployment and service as YAMLs, they would look like the following:

Deployment:
```
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: helloworld-deployment
spec:
  selector:
    matchLabels:
      app: helloworld
  replicas: 1 # tells deployment to run 1 pods matching the template
  template: # create pods using pod definition in this template
    metadata:
      labels:
        app: helloworld
    spec:
      containers:
      - name: helloworld
        image: karthequian/helloworld:latest
        ports:
        - containerPort: 80
```

Service:
```
apiVersion: v1
kind: Service
metadata:
  name: helloworld-service
spec:
  # if your cluster supports it, uncomment the following to automatically create
  # an external load-balanced IP for the frontend service.
  type: LoadBalancer
  ports:
  - port: 80
    protocol: TCP
    targetPort: 80
  selector:
    app: helloworld
```


### Execute our deployment using YAML

To create these, we can run the command `kubectl create -f helloworld-deployment.yml` to create our deployment and `kubectl create -f helloworld-service.yml` to create the service. This will take the contents of the YAML file and create the necessary components in the file.

```
devcfgc$ kubectl create -f helloworld-deployment.yml
deployment "helloworld-deployment" created
devcfgc$ kubectl create -f helloworld-service.yml
service "helloworld-service" created
```

Typically, in the real world, you would mostly not use seperate files to break up your application and would have it in a single file that encompasses the entire application with both the deployment and the service component. An example of this YAML file is shown here:

```
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: helloworld-deployment
spec:
  selector:
    matchLabels:
      app: helloworld
  replicas: 1 # tells deployment to run 1 pods matching the template
  template: # create pods using pod definition in this template
    metadata:
      labels:
        app: helloworld
    spec:
      containers:
      - name: helloworld
        image: karthequian/helloworld:latest
        ports:
        - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: helloworld-service
spec:
  # if your cluster supports it, uncomment the following to automatically create
  # an external load-balanced IP for the frontend service.
  type: LoadBalancer
  ports:
  - port: 80
    protocol: TCP
    targetPort: 80
  selector:
    app: helloworld
```

Notice the `---` that marks the end of one section and starts another.

### Verify that the application is working as expected
Finally, to see this new helloworld working as expected, we will run the minikube command to expose the service in the browser with the following command:

```
devcfgc$ minikube service helloworld-service
Opening kubernetes service default/helloworld-service in default browser...
```

### Learn how to scale the helloworld application

When we run the `kubectl get deployments` command, we notice that there's a single instance of the helloworld deployment running, as well as a single pod associated with the deployment.

To run a replica set for our helloworld deployment, run the command `kubectl scale --replicas=3 deploy/helloworld-deployment`. This will add three replicas for our deployment, which effectively means three pods running for a single deployment.

```
devcfgc$ kubectl scale --replicas=3 deploy/helloworld-deployment
deployment "helloworld-deployment" scaled
```

Now, if we run `kubectl get all`, we'll see three pods instead of one.

```
devcfgc$ kubectl get all
NAME                                        READY     STATUS    RESTARTS   AGE
po/helloworld-deployment-2148054017-6fc7f   1/1       Running   0          6h
po/helloworld-deployment-2148054017-88nd8   1/1       Running   0          53m
po/helloworld-deployment-2148054017-dvg47   1/1       Running   0          53m

NAME                     CLUSTER-IP   EXTERNAL-IP   PORT(S)        AGE
svc/helloworld-service   10.0.0.244   <pending>     80:32138/TCP   6h
svc/kubernetes           10.0.0.1     <none>        443/TCP        9d

NAME                           DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
deploy/helloworld-deployment   3         3         3            3           6h

NAME                                  DESIRED   CURRENT   READY     AGE
rs/helloworld-deployment-2148054017   3         3         3         6h
```
