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

