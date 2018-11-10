## 1 - RBAC / AAD

kubectl create -f ./1-rbac-aad/1-rbac.yaml
kubectl create -f ./1-rbac-aad/2-azure-vote.yaml

## 2 - Resource Controll

## 3 - Scheduling

kubectl label nodes aks-nodepool1-13681204-1 disktype=ssd
kubectl label nodes aks-nodepool1-13681204-2 disktype=ssd

kubectl taint nodes aks-nodepool1-13681204-0 dept=it:NoSchedule

## 4 - Priority

