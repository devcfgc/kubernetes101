# [ACS Engine](https://github.com/Azure/acs-engine/) #

## Deploy k8s cluster with acs-engine

## Generate the model
```
acs-engine generate --api-model deploy.json
```

## Deploy k8s into Azure
```
az login
az account set --subscription <SUBSCRIPTION_NAME>
az group create -n <RESOURCE_GROUP_NAME> -l westeurope
az group deployment create --name <DEPLOYMENT_NAME> -g <RESOURCE_GROUP_NAME> --template-file _output/deploy/azuredeploy.json --parameters _output/deploy/azuredeploy.parameters.json --verbose
```
