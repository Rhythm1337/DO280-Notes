# 1. Control Application Permissions with Security Context Constraints

## What you will learn
Create service accounts, apply permissions and manage security constraints

## Security Context Constraints (SCCs)
SCCs are a security mechanism that limits the access from a running pod in Open Shift to host enviroment. The SCCs control the following:
- Running privileged containers
- Change User ID
- Changing the SELinux context of a container
- Request extra capabilities for a container
- Use host directory as volumes

### To view the list of different SCCs, Type the following command

```
oc get scc
oc get scc -o name (will only show names)
```

The default SCC used by Open Shift **restricted**

```
oc describe scc restricted
```

## Viewing additional information related to SCC

```
oc describe scc hostnetwork

oc describe pod container-name -n namespace-name | grep scc
oc get deployment depolyment-name -o yaml | oc adm policy scc-subject-review -f -
```

## Creating a service account to override some security

```
oc create serviceaccount account-name
oc adm policy add-src-to-user SRC -Z account-name
oc set serviceaccount deployment/deployment-name
```

# 2.  Allow Application Access to Kubernetes APIs

## What you will learn
Run an application that requires access to Kubernetes API

## Securing Kubernetes APIs
With kubernetes API's, a user or an application can query and modify the cluster. To protect your cluster from malacious interactions, you must grant access to different Kubernetes APIs.

## Application Authorization with Service Accounts
A service account is a Kubernetes object within a project.

## Use Cases for Kubernetes API Access
- Monitoring Applications
- Controllers
- Operators

# Application Kubernetes API Authorization with Roles

## Binding Roles to Service Accounts

```
oc adm policy add-role-to-user cluster-role -z service-account
```

```
oc adm policy add-cluster-role-to-user cluster-role service-account
```

# 3. Cluster and Node Maintenance with Kubernetes Cron Jobs
## What you will learn
Automate regular tasks on the cluster with kubernetes cron jobs

## Kubernetes Batch API Resources

### Job
Kubernetes tasks that are executed once 

### Cron Job
Kubernetes tasks that are executed regularly/ on a schedule

## Kubernetes Jobs

```
oc create --dry-run=client -o yaml test --image=image_name --curl http://localhost:8080
```

## Kubernetes Cron Jobs

```
oc create cronjob --dry-run=client -o yaml test --image=image_name --schedule '0 0 * * *' --curl http:/localhost:8080
```

## Linux Cron Jobs
These are your normal cronjobs accessed via `crontab -e` or /etc/crontab

**You can also Automate Application Maintenance and General Maintenance Tasks with Cron Jobs. View the student guide for all the yaml files**

