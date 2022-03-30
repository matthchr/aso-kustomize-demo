# aso-kustomize-demo
A simple demo using Kustomize and Azure Service Operator to deploy the backend of a cool website

## Running the Demo

1. Get a Kubernetes cluster
2. Set up Azure Service Operator on your cluster: See https://azure.github.io/azure-service-operator/#installation
3. Create the dev deployment of the site `kubectl apply -k patches/dev`
4. Create the prod deployment of the site `kubectl apply -k patches/prod`

You can run the following commands to check on the status of the components:

List status:
`kubectl api-resources -o name | grep azure.com | paste -sd "," - | xargs kubectl get -A`

Watch status:
`watch "kubectl api-resources -o name | grep azure.com | paste -sd "," - | xargs kubectl get -A"`
