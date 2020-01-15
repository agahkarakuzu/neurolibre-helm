## Before install 

* Make sure that you have CRD for `cert-manager` `v0.11.1`. 

```
kubectl apply --validate=false -f https://raw.githubusercontent.com/jetstack/cert-manager/release-0.11/deploy/manifests/00-crds.yaml
```

With `cert-manager > v0.11.0`, use the following `API version` in `clusterissuer.yaml`: 

```
apiVersion: cert-manager.io/v1alpha2
```

and the following `ACME` server: 

`https://acme-staging-v02.api.letsencrypt.org/directory` 


* Make sure that you removed old CRDs belonging to an earlier version of `v.X.Y` 

kubectl delete -f https://raw.githubusercontent.com/jetstack/cert-manager/release-X.Y/deploy/manifests/00-crds.yaml

```
kubectl delete crd \
    certificates.certmanager.k8s.io \
    issuers.certmanager.k8s.io \
    clusterissuers.certmanager.k8s.io
```

## Installing the Chart

You need to have `config.yaml` and `secrets.yaml` in the same directory with 
the `neurolibre-chart` folder. You can find `config.yaml` and a template 
for `secrets.yaml` in the `deploy` folder. 


To install the chart:

```
cd neurolibre-chart 
```
* Update Helm dependencies described in `requirements.txt` 
```
helm dependency update 
```
* Create persistent volume 
```
kubectl apply -f pv.yaml
```
* Install 
```
cd ..
sudo helm install ./neurolibre-chart --name=neurolibre --namespace=binderhub -f config.yaml -f secret.yaml
```

## Upgrading the Chart

sudo helm upgrade neurolibre ./neurolibre-chart --namespace=binderhub -f config.yaml -f secret.yaml

## Packaging the Chart

To package the chart:
```
helm package neurolibre-chart
```

## To delete 
```
helm del --purge neurolibre
```


## Current limitations on our OpenStack API 

* We don't have a `LoadBalancer` API and implementation available. See more 
details [here](https://github.com/neurolibre/neurolibre-binderhub/issues/21#issuecomment-571642742).

This may or may not require setting up our own ingress controller. 

Then somehow change our configuration in that respect: 

https://zero-to-jupyterhub.readthedocs.io/en/latest/administrator/advanced.html#arbitrary-extra-code-and-configuration-in-jupyterhub-config-py

## References

* [alan-turing-institute/hub23-deploy](https://github.com/alan-turing-institute/hub23-deploy/tree/master/hub23-chart)

* https://discourse.jupyter.org/t/wip-documentation-about-cert-manager/2068

* https://binderhub.readthedocs.io/en/latest/https.html#cert-manager-for-automatic-tls-certificate-provisioning

* https://github.com/jupyterhub/zero-to-jupyterhub-k8s/issues/641




