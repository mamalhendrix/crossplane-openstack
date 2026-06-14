# crossplane-openstack
imagin we have multi tenant projects that needs kubernetes cluster for each tenant. 
we will use crossplane instead of terraform to create N-master and Y-workers on the openstack. with a fast and reliable way. so lets dig in :)

**1- we need a kubernetes cluster**
**2- install the crossplane addon:**

kubectl create namespace crossplane-system
helm repo add crossplane-stable https://charts.crossplane.io/stable
helm repo update
helm install crossplane --namespace crossplane-system crossplane-stable/crossplane

DOCUMENT:
**** https://artifacthub.io/packages/helm/crossplane/crossplane
------------------------------------------------------------------------------------------------------------------------------------------
3- we need to add two functions:

crossplane-contrib-function-go-templating
crossplane-contrib-function-patch-and-transform

3-1 install the **crossplane-contrib-function-go-templating**  (if you live in iran you should use any anti sancetions solutions , i used "BEGZAR" 185.55.226.26 , 185.55.225.25 , 185.55.224.24 )
****install the up cli

curl -sL https://cli.upbound.io | sh

up ctp function install xpkg.upbound.io/crossplane-contrib/function-go-templating:v0.12.1
up ctp function install xpkg.upbound.io/crossplane-contrib/function-patch-and-transform:v0.10.7

****check the installation:

kubectl get function

you should see:
NAME                                              INSTALLED   HEALTHY   PACKAGE                                                                      AGE
crossplane-contrib-function-go-templating         True        True      xpkg.crossplane.io/crossplane-contrib/function-go-templating:v0.12.1         108m
crossplane-contrib-function-patch-and-transform   True        True      xpkg.crossplane.io/crossplane-contrib/function-patch-and-transform:v0.10.7   6h17m

**install the openstack provider:**

kubectl apply -f 




