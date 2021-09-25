# Zero downtime evacuation of services from one node to another in EKS
This project documents the detailed steps to move pods and services from one node to another without disrupting the ongoing traffic in single EKS cluster.

## Prerequisites
1. AWS EKS cluster setup with Node groups
2. aws-cli
3. kubectl

## Instructions
1. Watch the status of the new node and wait for new node to join the cluster and reach the Ready status so that all Daemonsets are created and ready.
```
kubectl get nodes --watch
```

2. Once the new node is ready then taint the existing node so that scheduler don't schedule any new pods
```
kubectl taint nodes <node_name> key=value:NoSchedule
```

3. Now drain the deployments in the existing node. If HPA is configured then all the pods removed from the existing node will be scheduled in new node
```
kubectl drain --ignore-daemonsets --delete-local-data --grace-period=10 <node name>
```
