# w902
# What is it?
* A minimalistic app that shows the current temparatures in predefinde areas.
* A Resilent infrastructur for IoT Devices
## Requierments
* 3 Node Kuberbetes Cluster
* Helm
## Install Helm
* Get the Binary Build for helm
[https://github.com/kubernetes/helm/releases](https://github.com/kubernetes/helm/releases)
### Create a Tiller service-account
```
kubectl create -f rbac-config.yaml
```
### Initialize Helm
```
helm init --service-account tiller --canary-image
```
## Configure Ingress
```
helm install stable/nginx-ingress --namespace kube-system

kubectl get service -l app=nginx-ingress --namespace kube-system

```
# Configure Cert-Manager
```
helm install stable/cert-manager --set ingressShim.defaultIssuerName=letsencrypt-prod --set ingressShim.defaultIssuerKind=ClusterIssuer
```
## Create an Cert Issuer
kubectl create -f issuer.yml
## Creat a Cert Object
```
kubectl create -f cert-obj.yml
```
# Run the weather APP
insert your openweathermap api key
## Build the Docker container
docker build .
## Tag the container
```
docker tag <<container id>> <<user id>>/<<repository name>>:<<version>>
docker tag 0e5574283393 docker pull backzacc/w902:latest
```
## Push the container to docker hub
```
docker push <<user id>>/<<app name>>:<<version>>
docker push
```
## edit the image in the weather-deployment file
## Create the Deployment
```
kubectl create -f ingress-resource.yaml
```
## Create the ingress Resource
### Edit the hostname within the ingress resource
```

kubectl create -f ingress-resource.yaml
```
## visit the site to make sure it works
## Testing
* Delete a random Pod
