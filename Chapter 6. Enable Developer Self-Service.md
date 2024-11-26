# 1. Project and Cluster Quotas
## What you will learn
Configure resource usage, set limits project wide, etc.

## Limiting Workloads
Kubernetes clusters can run all kinds of workloads across many nodes. Clusters have limited resources and if they exceed unwanted problems may arise. For example if a cluster is configured to autoscale without limits, it might incur unwanted ecnonomic costs. To help with this issue kubernetes can limit/reserve resources.

### Resource Limits
This limits resources that a workload consumes and helps prevent workload from consuming excessive resources

### Resource Requests
Workloads can declare how much resources they need and kubernetes tracks requested resources and prevents deployment of new workloads if the cluster has insufficient resources.

