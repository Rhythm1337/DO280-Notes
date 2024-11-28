#  Control Application Permissions with Security Context Constraints

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
