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

With Edge termination, TLS termination occurs at the router meaning the traffic sent from the application to the router is unencrypted.

**Passthrough**

With Passthrough termination, encrypted traffic is directly sent to the pod without TLS termination at the router. This is common for when a client has to access the application directly.

**Re-encryption**

This is a variation of Edge termination where traffic sent to the router is encrypted and the route from the router to the application is also encrypted with might be encrypted with a different certificate.

## Securing Applications with Edge Routes

Before creating a encrypted route you need a TLS certificate

Command to create an encrypted edge route

```
--key contains the private key
--cert contains the signed certificate
```

If --key and --cert are omitted then OpenShift will provide a certificate from its internal Certificate Authority (CA). Use the command 

to view the provided certificate.

**View figure 4.2 for better understanding**

## Securing Applications with Passthrough Routes
Passthrough routes offer a more secure alternative, because the application exposes its TLS certificate. As such the traffic between client and application is fully encrypted. To create a passthrough route you need a certificate and a way for your application to access it. The best way is by using OpenShift TLS secrets. You can mount the secrets on the container to expose them. 

**View figure 4.3 for better understanding**

## Securing Applications with Re-encrypt Routes
Read student guide for this lol


# 2. Configure Network Policies

## Managing Network Policies in OpenShift

With network policies you can configure isolation for individual pods. Network policies don't require administrative privileges, and give developers more control over applications in their projects. You can use network policies to create logical ones in the SDN that map to your organization network zone. Benefit of this approach is that the location of this approach is that the location of the pods become irrelevant, because with network policies you can separate traffic regardless of where it originates from.

In contrast to traditional firewalls, Kubernetes network policies control network traffic between pods by using labels instead of IP addresses. To manage network communication between pods in two namespaces, assign a label to the namespace that needs access to another namespace and create a network policy that selects these labels.

**For all policies examples refer student guide**

TLDR Network policies control pod to pod traffic (only for pods that aren't using host networking). Traffic between pods that use host networking bypasses these policies on the same node.

# 3. Protect Internal Traffic with TLS

## Zero-trust Environments
Zero trust environments assume that every interaction begins at an untrusted state. Users can access only files and objects that they are assigned, communication must be encrypted and client applications must verify the authenticity of the servers

By default openshift encrypts traffic between nodes and the control plane, and prevents external entities from reading internal traffic. This encryption is stronger than default kubernetes which does not encrypt internal traffic.

## Service Certificate Creation

Generating a certificate

```
oc annotate service hello service.beta.openshift.io/serving-cert-secret-name=hello-secret
```

After the secret is generated you need to mount it in the application deployment.
>For clients to verify the validity of a certificate, the application needs the CA bundle that signed that certificate.

## Configuration Maps

```
oc annotate configmap ca-bundle service.beta.openshift.io/inject-cabundle=true
```

## Key Rotation
 The CA certificate is valid for 26 months by default and is automatically rotated after 13 months. During this grace period of 13 months, each pod that is configured to trust the original CA certificate must be restarted in some way. A service automatically injects the new CA bundle.

**You can also manually rotate the certificate**

```
oc delete secret/signing-key -n openshift-service-ca
```

## Alternatives to Service Certificates
Using a service mesh or the cert manager operator

