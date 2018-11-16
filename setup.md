# Setup

## Minikube

minikube start --kubernetes-version=v1.11.3

## AAD enabled cluster

RESOURCE_GROUP=WinOpsAKSDemoAAD
AKS_NAME=WinOpsAKSDemoAAD

az group create --name $RESOURCE_GROUP --location eastus

az aks create \
  --resource-group $RESOURCE_GROUP \
  --name $AKS_NAME \
  --generate-ssh-keys \
  --aad-server-app-id $(az keyvault secret show --name aksServerAppId --vault-name nepeterskv --query value -o tsv) \
  --aad-server-app-secret $(az keyvault secret show --name aksServerAppSecret --vault-name nepeterskv --query value -o tsv) \
  --aad-client-app-id $(az keyvault secret show --name aksClientAppId --vault-name nepeterskv --query value -o tsv) \
  --aad-tenant-id $(az keyvault secret show --name AzureTenantID --vault-name nepeterskv --query value -o tsv) \
  --kubernetes-version 1.11.3 \
  --node-count 3

  ## Demo prep

  - Update node names in ./3-scheduling/1-node-sticky.yaml
  - Update node names in ./3-scheduling/2-node-selector.yaml
  - Update node names in ./3-scheduling/3-taint-tollerations.yaml