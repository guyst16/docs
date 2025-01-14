---
title: Upgrade self-hosted Kubernetes installation
---
# Upgrade Run:ai 

## Preparations

=== "Connected"
    No preparation required.

=== "Airgapped" 
    * Ask for a tar file `runai-air-gapped-<new-version>.tar` from Run:ai customer support. The file contains the new version you want to upgrade to. `new-version` is the updated version of the Run:ai control plane.
    * Prepare the installation artifact as described [here](../preparations/#prepare-installation-artifacts) (untar the file and run the script to upload it to the local container registry). 


## Upgrading to Version 2.9

Before upgrading the control plane, run: 

``` bash
kubectl delete --namespace runai-backend --all \
    deployments,statefulset,svc,ing,ServiceAccount,secrets
```

Prior to version 2.9, the Run:ai installation, by default, has also installed NGINX. It was possible to disable this installation. if NGINX is disabled in your current installation then __do not__ run the following 2 lines. 

``` bash
kubectl delete ValidatingWebhookConfiguration runai-backend-nginx-ingress-admission
kubectl delete ingressclass nginx 
```

Next, install NGINX as described [here](../../cluster-setup/cluster-prerequisites.md#ingress-controller)

Then upgrade the control plane as described in the next section. 

## Upgrade Control Plane


Run the helm command below. 

=== "Connected"
    ```
    helm repo add runai-backend https://backend-charts.storage.googleapis.com
    helm repo update
    helm get values runai-backend -n runai-backend > be-values.yaml
    helm upgrade runai-backend -n runai-backend runai-backend/runai-backend -f be-values.yaml 
    ```
=== "Airgapped"
    ```
    helm get values runai-backend -n runai-backend > be-values.yaml
    helm upgrade runai-backend runai-backend/runai-backend-<version>.tgz -n \
        runai-backend  -f be-values.yaml
    ```
    (replace `<version>` with the control plane version)


## Upgrade Cluster 



=== "Connected"
    To upgrade the cluster follow the instructions [here](../../cluster-setup/cluster-upgrade.md).

=== "Airgapped"
    ```
    kubectl apply -f runai-crds.yaml
    helm get values runai-cluster -n runai > values.yaml
    helm upgrade runai-cluster -n runai runai-cluster-<version>.tgz -f values.yaml
    ```
    (replace `<version>` with the cluster version)
