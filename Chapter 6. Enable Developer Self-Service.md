# 1. Project and Cluster Quotas
## What you will learn
Configure resource usage, set limits project wide, etc.

## Limiting Workloads
Kubernetes clusters can run all kinds of workloads across many nodes. Clusters have limited resources and if they exceed unwanted problems may arise. For example if a cluster is configured to autoscale without limits, it might incur unwanted ecnonomic costs. To help with this issue kubernetes can limit/reserve resources.

### Resource Limits
This limits resources that a workload consumes and helps prevent workload from consuming excessive resources

### Resource Requests
Workloads can declare how much resources they need and kubernetes tracks requested resources and prevents deployment of new workloads if the cluster has insufficient resources.

## Resource Quotas

```
apiVersion: v1
kind: ResourceQuota
metadata:
  name: limit-memory
  namespace: test
spec:
  hard:
    limits.memory: 4Gi
    requests.memory: 2Gi
  scopes: {}
  scopeSelector: {}
```

```
oc api-resources --test-group=""  --namespaced=true
```

## Applying Project Quotas

```
oc create resourcequota test --hard=count/pods=1
```

```
apiVersion: v1
kind: ResourceQuota
metadata:
  name: test
spec:
  hard:
    count/pods: "1"
```

```
oc get quota example -o yaml
```

**Common Command**

```
oc get quota
```

# 2. Per-Project Resource Constraints: Limit Ranges

## What you will learn
Configure default maximum resources for pods per project

## Managing Namespace Resources
Cluster 


# 3. The Project Template and the Self-Provisioner Role

**Read Book for rest**  
