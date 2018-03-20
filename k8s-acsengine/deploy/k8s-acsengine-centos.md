# Generate [acs-engine](https://github.com/Azure/acs-engine/blob/master/docs/kubernetes/deploy.md#acs-engine-the-long-way) model
```bash
acs-engine generate --api-model k8s-acsengine-centos.json
```

# Deploy kubernetes cluster
```bash
$ az login

$ az account set --subscription "<SUBSCRIPTION NAME OR ID>"

$ az group create \
    --name "<RESOURCE_GROUP_NAME>" \
    --location "<LOCATION>"

$ az group deployment create \
    --name "<DEPLOYMENT NAME>" \
    --resource-group "<RESOURCE_GROUP_NAME>" \
    --template-file "./_output/<INSTANCE>/azuredeploy.json" \
    --parameters "./_output/<INSTANCE>/azuredeploy.parameters.json"
```
