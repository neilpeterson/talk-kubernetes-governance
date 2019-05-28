# Demo 1: RBAC / AAD
kubectl get pods
kubectl create -f ./1-rbac-aad/1-rbac.yaml
kubectl create -f ./1-rbac-aad/2-azure-vote.yaml
kubectl create -f ./1-rbac-aad/2-azure-vote.yaml --namespace lob
kubectl delete -f ./1-rbac-aad/2-azure-vote.yaml --namespace lob

# Demo 2: Resource Controll
# Simple resource request and limit
kubectl create -f 2-resource-controll/1-sample-resquest-limit.yaml
kubectl describe pod azure-vote-back-sample
kubectl delete -f 2-resource-controll/1-sample-resquest-limit.yaml

# Resource quota and two pods, one will fail
kubectl create -f 2-resource-controll/2-resource-quota.yaml

# Add a second pod to namespace, this will also fail
kubectl create -f 2-resource-controll/3-second-resource-quota-pod.yaml

# Resource limit range
kubectl create -f 2-resource-controll/4-resource-limits.yaml
kubectl describe pod nepetersv1-no-limits --namespace pink

# Clean up
kubectl delete -f ./2-resource-controll

# Demo 3: Scheduling
# Node sticky
kubectl create -f 3-scheduling/1-node-sticky.yaml
kubectl get pods -o wide
kubectl delete -f 3-scheduling/1-node-sticky.yaml

# Node selector
kubectl label nodes aks-nodepool1-35171595-0 disktype=ssd
kubectl label nodes aks-nodepool1-35171595-1 disktype=ssd
kubectl describe node aks-nodepool1-35171595-0
kubectl create -f 3-scheduling/2-node-selector.yaml
kubectl get pods -o wide
kubectl delete -f 3-scheduling/2-node-selector.yaml
kubectl label nodes aks-nodepool1-35171595-0 disktype-
kubectl label nodes aks-nodepool1-35171595-1 disktype-

# Taint / Tollerations
kubectl taint nodes aks-nodepool1-35171595-0 dept=it:NoSchedule
kubectl describe node aks-nodepool1-35171595-0
kubectl create -f 3-scheduling/3-taint-tolleration.yaml
kubectl get pods -o wide
kubectl delete -f 3-scheduling/3-taint-tolleration.yaml
kubectl taint nodes aks-nodepool1-35171595-0 dept-

# 4: Priority
# Priority class
kubectl create -f 4-priority/1-priority-class.yaml

# Slam workload
kubectl create -f 4-priority/2-slam-workload.yaml
kubectl get pods

# Atempt to start pod with no priority
kubectl create -f 4-priority/3-workload-no-priority.yaml
kubectl get pods
kubectl describe pod nepetersv1-no-priority

# Start pod with high priority
kubectl create -f 4-priority/4-workload-with-priority.yaml
kubectl get pods