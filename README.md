# aso-kustomize-demo
A simple demo using Kustomize and Azure Service Operator to deploy the backend of a TODO website.

Adapted from https://github.com/Azure-Samples/azure-service-operator-samples/blob/master/cosmos-todo-list-mi.

## Running the Demo

1. Get a Kubernetes cluster
2. Set up Azure Service Operator on your cluster: See https://azure.github.io/azure-service-operator/#installation
3. Create the dev deployment of the site `kustomize build overlays/dev | kubectl apply -f -`
4. Create the prod deployment of the site `kustomize build overlays/prod | kubectl apply -f -`

You can run the following commands to check on the status of the components:

List status:
`kubectl api-resources -o name | grep azure.com | paste -sd "," - | xargs kubectl get -A`

or namespace scoped:
`kubectl api-resources -o name | grep azure.com | paste -sd "," - | xargs kubectl get -n todo-prod`

Watch status:
`watch "kubectl api-resources -o name | grep azure.com | paste -sd "," - | xargs kubectl get -A"`

Port forward:
kubectl port-forward -n todo-prod service/todo-service 8080:80


## Installign ASO
```sh
helm repo add aso2 https://raw.githubusercontent.com/Azure/azure-service-operator/main/v2/charts
helm upgrade --install aso2 aso2/azure-service-operator \
    --create-namespace \
    --namespace=azureserviceoperator-system \
    --set azureSubscriptionID=$AZURE_SUBSCRIPTION_ID \
    --set azureTenantID=$AZURE_TENANT_ID \
    --set azureClientID=$AZURE_CLIENT_ID \
    --set azureClientSecret=$AZURE_CLIENT_SECRET \
    --set crdPattern='resources.azure.com/*;documentdb.azure.com/*;managedidentity.azure.com/*'
```
