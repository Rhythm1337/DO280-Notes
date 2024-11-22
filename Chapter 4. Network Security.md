# 1. Protect External Traffic with TLS

## What you will learn:
Allow and protect network connections to the application to the OpenShift cluster.

## Accessing Applications from External Networks
OpenShift offers many ways to expose your application to external networks. You can expose HTTP and HTTPS traffic, TCP and non-TCP traffic. 

### Some of these methods are:

**Service Types**
- NodePort
- Load Balancer

**API Resources**
- Ingress
- Route

**View Figure 4.1 to understand how the application is exposed**
>For performance reasons, routers send requests directly to pods based on service configuration.

## Encrypting Routes
Routes can be both encrypted or unencrypted. 

### OpenShift Route Encryption:

**Edge**

With Edge termination, TLS termination occours at the router meaning the traffic sent from the application to the router is unencrypted.

**Passthrough**

With Passthrough termination, encrypted traffic is directly sent to the pod without TLS termination at the router. This is common for when a client has to access the appliction directly.

**Re-encryption**

This is a variation of Edge termination where traffic sent to the router is encrypted and the route from the router to the application is also encrypted with might be encrypted with a different certificate.

## Securing Applications with Edge Routes

Before creating a encrypted route you need a TLS certificate

Command to create  an encrypted edge route

```
oc create route edge --service app-frontend --hostname api.apps.acme.com --key app.key --cert app.crt
```

--key contains the private key
--cert contains the signed certificate

If --key and --cert are ommited then OpenShift will provide a certificate form its internal Certificate Authority (CA). Use the command 

```
oc get secrets/router-ca -n openshift-ingress-operator -o yaml
```
to view the provided certificate.

**View figure 4.2 for better understanding**

## Securing Applications with Passthrough Routes
Passthrough routes offer a more secure alternative, because the application exposes it TLS certificate. As such the traffic between client and application is fully encrypted. To create a psssthorugh route you need a certificate and a way for your application ot access it. The best way is by using OpenShift TLS secrets. You can mount the secrets on the container to expose them. 

**View figure 4.3 for better understanding**

## Securing Applications with Re-encrypt Routes
Read student guide for this lol


# 2. Configure Network Policies

## Managing Network Policies in OpenShift

With network policies you can configure isolation for individual pods. Network policies dont require administrative privileges, and give developers more control over applications in their projects. You can use network policies to create logical ones in the SDN that map to your organization network zone. Benifit of this approach is that the location of this approach is that the location of the pods become irrelevant, because with network policies you can seperate traffic regardless of where it orignates form.

In contrast to traditional firewalls, Kubernetes network policies control network traffic between pods by using labels insted of ip addresses. To manage network communication between pods in two namespaces, assign a lable to the namespace that needs access to another namespace and create a network policy that selects these lables.

**For all policies examples refer student guide**

TLDR Network policies control pod to pod traffic(only for pods that arent using host networking). Traffic between pods that use host networking bypasses these policies on the same node.

# 3. Protect Internal Traffic with TLS

## Zero-trust Environments
