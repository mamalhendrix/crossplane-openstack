# crossplane-openstack
imaging we have multi tenant projects that needs kubernetes cluster for each tenant.
we will use crossplane instead of terraform to create N-masters and Y-workers on the openstack. with a fast and reliable way. so lets dig in :)

**1- we need a kubernetes cluster**

**2- install the crossplane addon:**

kubectl create namespace crossplane-system

helm repo add crossplane-stable https://charts.crossplane.io/stable

helm repo update

helm install crossplane --namespace crossplane-system crossplane-stable/crossplane

DOCUMENT:

**** https://artifacthub.io/packages/helm/crossplane/crossplane

------------------------------------------------------------------------------------------------------------------------------------------
3- install crossplane cli

curl -sfL "https://cli.crossplane.io/install.sh" | sh

DOCUMENT:
**https://docs.crossplane.io/cli/latest/**

------------------------------------------------------------------------------------------------------------------------------------------

4- **install the provider-openstack:**

kubectl apply -f provider-openstack.yaml

**check installation:**

kubectl get providers

kubectl get providerrevisions

------------------------------------------------------------------------------------------------------------------------------------------

**5- install functions:

**crossplane-contrib-function-go-templating** and **crossplane-contrib-function-patch-and-transform** . the easiest way is to use up cli tool**

**(if you live in iran you should use any anti sancetion solutions , i used "BEGZAR" 185.55.226.26 , 185.55.225.25 , 185.55.224.24 )**

curl -sL https://cli.upbound.io | sh

up ctp function install xpkg.upbound.io/crossplane-contrib/function-go-templating:v0.12.1

up ctp function install xpkg.upbound.io/crossplane-contrib/function-patch-and-transform:v0.10.7

**check the installation:**

kubectl get function

**you should see:**

<img width="1670" height="108" alt="image" src="https://github.com/user-attachments/assets/0c2cb87b-0bd9-4c6a-aeb0-2b8c9556191b" />


------------------------------------------------------------------------------------------------------------------------------------------

**6- edit and update the config.json with your openstack credentials and create a secret.**

kubectl create secret generic provider-openstack-config   --from-file=config=config.json   --namespace crossplane-system

------------------------------------------------------------------------------------------------------------------------------------------

**7-create composition resource definition:**

kubectl apply -f CompositeResourceDefinition.yaml

kubectl get CompositeResourceDefinition

------------------------------------------------------------------------------------------------------------------------------------------

**8-create composition:** (XRD)

kubectl apply -f composition.yaml

kubectl get composition

------------------------------------------------------------------------------------------------------------------------------------------

**9- claim the tenant**

kubectl apply -f tenant-claim.yaml

kubectl get instancev2s

------------------------------------------------------------------------------------------------------------------------------------------
**usefull commands:**

kubectl get provider -n crossplane-system
kubectl get instancev2s
kubectl get SubnetV2
kubectl get NetworkV2
kubectl get RouterV2
kubectl get crds | grep openstack

kubectl get providers
kubectl get ProviderConfig
kubectl get composition
kubectl get function

------------------------------------------------------------------------------------------------------------------------------------------
IMPORTANT NOTES:

**i used ubuntu-cloud-init image in the openstack named ubuntu-init , change the imageName section in the composition.yaml with your own image name.**

**change the user and password of the created vm in userData section in the composition.yaml in the.**

**add metadata to your cloud-init image with the key and value:**

img_config_drive:mandatory
