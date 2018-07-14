# w902
# Overview
## What is it?
* A minimalistic app that shows the current temparatures in predefinde areas.
* The configuration files of the Kubernetes infrastructure used in this Project.
# Why Kubernetes
We use Kubernetes, for the infrastructur because it is something new for us.
# Why not just using Serverless Containers?
Using Serveless Containers would have been easier and less fun.
## Requirements
* 3 Node Kuberbetes Cluster (We use Azure as Cloud Providor)
* Helm (Configured with RBAC)
# Install Helm
We have to install helm for the installatione of Ingress and the Cert-Manager
* Install the Binary Build
[https://github.com/kubernetes/helm/releases](https://github.com/kubernetes/helm/releases)
## Create a Tiller service-account
The Tiller service account is needed for to grant helm the necessary permissions.
```
kubectl create -f rbac-config.yaml
```
## Initialize Helm
The flag --canary-image allows us to use the testing version of helm.
It isn't recommended for deployment in production.
```
helm init --service-account tiller --canary-image
```
# Configure Ingress
Ingress is our gatway to the WWW.
```
helm install stable/nginx-ingress --namespace kube-system
kubectl get service -l app=nginx-ingress --namespace kube-system
```
# Configure Cert-Manager
The Cert-Manager is the Application responsible for requesting TSL certificates for your Domain.
```
helm install stable/cert-manager --set ingressShim.defaultIssuerName=letsencrypt-prod --set ingressShim.defaultIssuerKind=ClusterIssuer
```
## Create an Cert Issuer
The Cert issuer is used to supply the Cert Manger with the definition of your E-Mail and The CA used. We use Let's Encrypt because it's free. You need to change the used E-Mail befor you creat this Resource.
```
kubectl create -f issuer.yml
```
## Creat a Cert Object
The Cert Object tells the Cert-Manager which certificats needs to be Issued.
Change the Host within this file.
```
kubectl create -f cert-obj.yml
```
# Run the Weather-APP
The Weather-APP is a php script that request the current temparatur for predefined areas.
Insert your openweathermap api key into the PHP-Site (src/index.php).
## Build the Docker container
We need for the deployment in Kubernetes a Docker Container.
We use Automatic Builds for this Project, but building it maually and pushing it to a repo is another option.
## Create the Deployment
Edit the image name in the weather-deployment.yaml file and then create the resource.
```
kubectl create -f weather-deployment.yaml
```
## Create the Ingress resource
Edit the host within the ingress resource and the creat it.
```
kubectl create -f ingress-resource.yaml
```
## visit the site to make sure it works
https://yoursite.com
