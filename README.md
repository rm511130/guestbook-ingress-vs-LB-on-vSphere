# Demo 1

## Pre-requisites:

- You have access to a TKGI (PKS) environment deployed on vSphere using NSX-T
- You have access to a `small` cluster on this TKGI (PKS) environment
- The `small` cluster has not been used for any purpose and does not have a storage-class defined

## Demo Steps

1. First we log into your PKS environment as a `pks_admin` user. We find that we already have a `small` cluster provisioned.

```
pks login -a https://api.run.haas-257.pez.pivotal.io -p password -k -u pks_admin
pks cluster small
```
```python
PKS Version:              1.7.0-build.26
Name:                     small
K8s Version:              1.16.7
Plan Name:                small
UUID:                     994b5956-15d2-4fbb-b791-a5624e3347bb
Last Action:              CREATE
Last Action State:        succeeded
Last Action Description:  Instance provisioning completed
Kubernetes Master Host:   small.run.haas-257.pez.pivotal.io
Kubernetes Master Port:   8443
Worker Nodes:             1
Kubernetes Master IP(s):  10.195.11.128
Network Profile Name:
```
```
pks get-credentials small
kubectl cluster-info
```
```
Kubernetes master is running at https://small.run.haas-257.pez.pivotal.io:8443
CoreDNS is running at https://small.run.haas-257.pez.pivotal.io:8443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
```

2. We clone [this](https://github.com/rm511130/guestbook-ingress-vs-LB-on-vSphere) repo on a MacBook:

```
cd /work
git clone https://github.com/rm511130/guestbook-ingress-vs-LB-on-vSphere
cd guestbook-ingress-vs-LB-on-vSphere
```

3. Let's check that there isn't a storage class and then let's create one:

```
kubectl get sc
kubectl apply -f thin-storageclass.yaml
```
```
storageclass.storage.k8s.io/thin-disk created
```
```
kubectl describe sc thin-disk
```
```
Name:            thin-disk
IsDefaultClass:  Yes
Annotations:     kubectl.kubernetes.io/last-applied-configuration={"apiVersion":"storage.k8s.io/v1","kind":"StorageClass","metadata":{"annotations":{"storageclass.kubernetes.io/is-default-class":"true"},"name":"thin-disk"},"parameters":{"diskformat":"thin"},"provisioner":"kubernetes.io/vsphere-volume"}
,storageclass.kubernetes.io/is-default-class=true
Provisioner:           kubernetes.io/vsphere-volume
Parameters:            diskformat=thin
AllowVolumeExpansion:  <unset>
MountOptions:          <none>
ReclaimPolicy:         Delete
VolumeBindingMode:     Immediate
Events:                <none>
```

4. 



# Guestbook Kubernetes Deployment

This manifest provides a simple deployment of the Guestbook application for testing and validtion of your Kubernetes install.  It uses individual YAML files to deploy the namespace, storageclass, persistent volume claims and application components.  You then have an option to expose the Frontend application services using ingress or service type loadbalancer.  

Choose only one option to expose the Frontend service (i.e either guestbook-frontend-http-ingress.yaml, guestbook-frontend-https-ingress.yaml (requires you create guestbook-secret of time) or guestbook-frontend-svc-lb.yaml)
