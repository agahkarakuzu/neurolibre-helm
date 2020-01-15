




kubectl logs neurolibre-nginx-ingress-controller-

```
-------------------------------------------------------------------------------
NGINX Ingress controller
  Release:       0.27.0
  Build:         git-77ddda7f6
  Repository:    https://github.com/kubernetes/ingress-nginx
  nginx version: nginx/1.17.7

-------------------------------------------------------------------------------
======================================== ERROR 1 =======================================
...
W0115 10:56:25.897944       6 backend_ssl.go:46] Error obtaining X.509 certificate: unexpected error creating SSL Cert: no certificate PEM data found, make sure certificate content starts with 'BEGIN CERTIFICATE'
....
======================================== ERROR 2 =======================================
W0115 10:56:29.805116       6 controller.go:1125] Error getting SSL certificate "binderhub/neurolibre-binder-tls": local SSL certificate binderhub/neurolibre-binder-tls was not found. Using default certificate
```
*** 

## ClusterIssuer 

```
API Version:  cert-manager.io/v1alpha2
Kind:         ClusterIssuer
Spec:
  Acme:
    Email:  
    Private Key Secret Ref:
      Name:  staging-acme-key
    Server:  https://acme-staging-v02.api.letsencrypt.org/directory
    Solvers:
      Http 01:
        Ingress:
          Class:  nginx
Status:
  Acme:
    Last Registered Email:  redacted@example.com
    Uri:                    https://acme-staging-v02.api.letsencrypt.org/acme/acct/redacted
  Conditions:
    Last Transition Time:  2020-01-15T10:57:29Z
    Message:               The ACME account was registered with the ACME server
    Reason:                ACMEAccountRegistered
    Status:                True
    Type:                  Ready
Events:                    <none>
```

## Certificate  

```
API Version:  cert-manager.io/v1alpha2
Kind:         Certificate
Metadata:
  Creation Timestamp:  2020-01-15T10:57:28Z
  Generation:          1
  Owner References:
    API Version:           extensions/v1beta1
    Block Owner Deletion:  true
    Controller:            true
    Kind:                  Ingress
    Name:                  binderhub
    UID:                   redacted
  Resource Version:        381255
  Self Link:               /apis/cert-manager.io/v1alpha2/namespaces/binderhub/certificates/neurolibre-binder-tls
  UID:                     redacted
Spec:
  Dns Names:
    binder.conp.cloud
  Issuer Ref:
    Group:      cert-manager.io
    Kind:       ClusterIssuer
    Name:       staging
  Secret Name:  neurolibre-binder-tls
Status:
  Conditions:
    Last Transition Time:  2020-01-15T10:57:28Z
    Message:               Waiting for CertificateRequest "neurolibre-binder-tls-1850857680" to complete
    Reason:                InProgress
    Status:                False
    Type:                  Ready
Events:
  Type    Reason     Age   From          Message
  ----    ------     ----  ----          -------
  Normal  Requested  50m   cert-manager  Created new CertificateRequest resource "neurolibre-binder-tls-1850857680"
```

## Order 

```
API Version:  acme.cert-manager.io/v1alpha2
Kind:         Order
Metadata:
  Creation Timestamp:  2020-01-15T10:57:29Z
  Generation:          1
  Owner References:
    API Version:           cert-manager.io/v1alpha2
    Block Owner Deletion:  true
    Controller:            true
    Kind:                  CertificateRequest
    Name:                  redacted
    UID:                   redacted
  Resource Version:        381269
  Self Link:               /apis/acme.cert-manager.io/v1alpha2/namespaces/binderhub/orders/neurolibre-binder-tls-1850857680-3390848920
  UID:                     redacted
Spec:
  Csr:  redacted (a long hash)
  Dns Names:
    binder.conp.cloud
  Issuer Ref:
    Group:  cert-manager.io
    Kind:   ClusterIssuer
    Name:   staging
Status:
  Authorizations:
    Challenges:
      Token:     redacted
      Type:      http-01
      URL:       https://acme-staging-v02.api.letsencrypt.org/acme/...
      Token:     redacted
      Type:      dns-01
      URL:       https://acme-staging-v02.api.letsencrypt.org/acme/chall-v3/32872380/
      Token:     redacted
      Type:      tls-alpn-01
      URL:       https://acme-staging-v02.api.letsencrypt.org/acme/chall-v3/32872380
    Identifier:  binder.conp.cloud
    URL:         https://acme-staging-v02.api.letsencrypt.org/acme/authz-v3/
    Wildcard:    false
  Finalize URL:  https://acme-staging-v02.api.letsencrypt.org/acme/finalize/
  State:         pending
  URL:           https://acme-staging-v02.api.letsencrypt.org/acme/order/
Events:
  Type    Reason   Age   From          Message
  ----    ------   ----  ----          -------
  Normal  Created  50m   cert-manager  Created Challenge resource "neurolibre-binder-tls-1850857680-3390848920-820780268" for domain "binder.conp.cloud"
```

## Challenge 

```
API Version:  acme.cert-manager.io/v1alpha2
Kind:         Challenge
Metadata:
  Creation Timestamp:  2020-01-15T10:57:30Z
  Finalizers:
    finalizer.acme.cert-manager.io
  Generation:  1
  Owner References:
    API Version:           acme.cert-manager.io/v1alpha2
    Block Owner Deletion:  true
    Controller:            true
    Kind:                  Order
    Name:                  redacted
    UID:                   redacted
  Resource Version:        381307
  Self Link:               /apis/acme.cert-manager.io/v1alpha2/namespaces/binderhub/challenges/...
  UID:                     redacted
Spec:
  Authz URL:  https://acme-staging-v02.api.letsencrypt.org/acme/authz-v3/...
  Dns Name:   binder.conp.cloud
  Issuer Ref:
    Group:  cert-manager.io
    Kind:   ClusterIssuer
    Name:   staging
  Key:      redacted
  Solver:
    Http 01:
      Ingress:
        Class:  nginx
  Token:        redacted
  Type:         http-01
  URL:          https://acme-staging-v02.api.letsencrypt.org/acme/chall-v3/...
  Wildcard:     false
Status:
  Presented:   true
  Processing:  true
 ======================================== ERROR 3 =======================================
  Reason:      Waiting for http-01 challenge propagation: failed to perform self check GET request 'http://binder.conp.cloud/.well-known/acme-challenge/redacted': Get http://binder.conp.cloud/.well-known/acme-challenge/redacted: dial tcp: lookup binder.conp.cloud on 10.96.0.10:53: no such host
 
  State:       pending
Events:
  Type    Reason     Age   From          Message
  ----    ------     ----  ----          -------
  Normal  Started    50m   cert-manager  Challenge scheduled for processing
  Normal  Presented  50m   cert-manager  Presented challenge using http-01 challenge mechanism
  ```

## Services 

```
  NAME                                       TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)                      AGE
binder                                     ClusterIP      10.107.108.126   <none>        80/TCP                       53m
cm-acme-http-solver-h5l2p                  NodePort       10.99.103.16     <none>        8089:31940/TCP               52m
hub                                        ClusterIP      10.106.140.195   <none>        8081/TCP                     53m
neurolibre-cert-manager                    ClusterIP      10.104.216.134   <none>        9402/TCP                     53m
neurolibre-cert-manager-webhook            ClusterIP      10.109.209.12    <none>        443/TCP                      53m
neurolibre-nginx-ingress-controller        LoadBalancer   10.97.87.165     <pending>     80:32283/TCP,443:31352/TCP   53m
neurolibre-nginx-ingress-default-backend   ClusterIP      10.99.47.187     <none>        80/TCP                       53m
proxy-api                                  ClusterIP      10.105.168.107   <none>        8001/TCP                     53m
proxy-public                               LoadBalancer   10.97.21.138     <pending>     443:32568/TCP,80:32057/TCP   53m
```