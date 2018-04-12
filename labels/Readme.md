# Adding labels to the application

### Adding labels during build time

You can add labels to pods, services and deployments either at build time or at run time. If you're adding labels at build time, you can add a label section in the metadata portion of the YAML as shown below:

```
apiVersion: v1
kind: Pod
metadata:
  name: helloworld
  labels:
    env: production
    author: karthequian
    application_type: ui
    release-version: "1.0"
spec:
  containers:
  - name: helloworld
    image: karthequian/helloworld:latest
```

You can deploy the code above by using the command `kubectl create -f helloworld-pod-with-labels.yml`

### Viewing labels

`kubectl get pods --show-labels`.

### Adding labels to running pods
`kubectl label po/helloworld app=helloworld`. This adds the label `app` with the value `helloworld` to the pod.

To update the value of a label, use the `--overwrite` flag in the command as follows: `kubectl label po/helloworld app=helloworldapp --overwrite`

### Deleting a label
To remove an existing label, just add a `-` to the end of the label key as follows: `kubectl label po/helloworld app-`. This will remove the app label from the helloworld pod.

### Searching by labels
- You can search for labels with the flag `--selector` (or `-l`). If you want to search for all the pods that are running in production, you can run
`kubectl get pods --selector env=production`
- You can also do more complicated searches, like finding any pods owned by Karthik in the development tier, by the following query `dev-lead=karthik,env=staging`:
`kubectl get pods -l dev-lead=karthik,env=staging`
- Or, any apps not owned by Karthik in staging (using the ! construct):
`kubectl get pods -l dev-lead!=karthik,env=staging`
- Querying also supports the `in` keyword
`kubectl get pods -l 'release-version in (1.0,2.0)'`
- Or a more complicated example:
`kubectl get pods -l "release-version in (1.0,2.0),team in (marketing,ecommerce)"`
- The opposite of "in" is "notin". Surprise. "Notin" is supported as well, as shown in this example:
`kubectl get pods -l 'release-version notin (1.0,2.0)'`

### Extending the label concept to deployments/services

Labels will also work with `kubectl get services/deployments/all --show-labels` and will return labels for your services, deployments or all objects.
`kubectl get all --show-labels`

And, you can delete pods, services or deployments by label as well! For example `kubectl delete pods -l dev-lead=karthik` will delete all pods who's dev-lead was Karthik.
