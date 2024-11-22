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

If --key and --cert are ommited 
