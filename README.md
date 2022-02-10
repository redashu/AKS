# AKS

## REvision --

## CLoud controller manager / kube controller manager 

<img src="cloudc.png">

### micro service based ingress and deployment with HPA understaning 

<img src="micro.png">

### before deploy ingress controlle i am cleaning up  my deployment 

```
kubectl delete all --all
pod "init-container-example-1" deleted
pod "pod1" deleted
pod "web" deleted
service "httpd" deleted
service "my-headless-service" deleted
service "nginx" deleted
service "web" deleted
fire@ashutoshhs-MacBook-Air ~ % kubectl  get ns
NAME                   STATUS   AGE
ashu-space             Active   24h
calico-system          Active   25h
default                Active   25h
kube-node-lease        Active   25h
kube-public            Active   25h
kube-system            Active   25h
kubernetes-dashboard   Active   22h
securens               Active   17h
sumair-space           Active   11m
tigera-operator        Active   25h
fire@ashutoshhs-MacBook-Air ~ % kubectl  delete ns  kubernetes-dashboard  
namespace "kubernetes-dashboard" deleted

```

### Deploy nginx ingress controller in AKS  from azure shell 

```
 helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
 helm repo update 
 helm install ingress-nginx ingress-nginx/ingress-nginx --create-namespace --namespace ingress-nginx
 
```

### Ingress watch 

<img src="ingresswatch.png">

## Service in k8s

<img src="svcall.png">

### pushing image in ACR 

```
docker  tag  dockerashu/nodeapp:v1   ashutoshh.azurecr.io/pwcnode:appv1 
[sumair@ip-172-31-84-215 ~]$ 
[sumair@ip-172-31-84-215 ~]$ docker login   ashutoshh.azurecr.io
Username: ashutoshh
Password: 
WARNING! Your password will be stored unencrypted in /home/sumair/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded
[sumair@ip-172-31-84-215 ~]$ docker  push  ashutoshh.azurecr.io/pwcnode:appv1 
The push refers to repository [ashutoshh.azurecr.io/pwcnode]
a437cf5b3db0: Pushing [==================================================>]  201.3MB
11fc80ef4d26: Pushed 
93743162602b: Pushed 
f943c8afb872: Pushed 
ccaeecdb
```

### deploying private image from ACR --

```
fire@ashutoshhs-MacBook-Air yamls % kubectl apply -f  ashunodeapp.yaml 
deployment.apps/nodepapp created
fire@ashutoshhs-MacBook-Air yamls % 
fire@ashutoshhs-MacBook-Air yamls % kubectl  get  deploy 
NAME       READY   UP-TO-DATE   AVAILABLE   AGE
nodepapp   0/1     1            0           12s
fire@ashutoshhs-MacBook-Air yamls % kubectl  get  po     
NAME                       READY   STATUS             RESTARTS   AGE
nodepapp-d976b48b7-lkp2p   0/1     ImagePullBackOff   0          21s
fire@ashutoshhs-MacBook-Air yamls % kubectl  get  po -w
NAME                       READY   STATUS             RESTARTS   AGE
nodepapp-d976b48b7-lkp2p   0/1     ImagePullBackOff   0          29s
nodepapp-d976b48b7-lkp2p   0/1     ErrImagePull       0          32s

```

### creating secret for registry cred 

```
 kubectl  create  secret 
Create a secret using specified subcommand.

Available Commands:
  docker-registry Create a secret for use with a Docker registry
  generic         Create a secret from a local file, directory, or literal value
  tls             Create a TLS secret

Usage:
  kubectl create secret [flags] [options]

Use "kubectl <command> --help" for more information about a given command.
Use "kubectl options" for a list of global command-line options (applies to all commands).
fire@ashutoshhs-MacBook-Air ~ % kubectl  create  secret  docker-registry   acrsec  --docker-server=ashutoshh.azurecr.io       --docker-username=ashutoshh  --docker-password="bWlEAidI0qBuJ9zlIK=Twczq4wPzoMT+"  --dry-run=client -o yaml 
apiVersion: v1
data:
  .dockerconfigjson: eyJhdXRocyI6eyJhc2h1dG9zaGguYXp1cmVjci5pbyI6eyJ1c2VybmFtZSI6ImFzaHV0b3NoaCIsInBhc3N3b3JkIjoiYldsRUFpZEkwcUJ1Sjl6bElLPVR3Y3pxNHdQem9NVCsiLCJhdXRoIjoiWVhOb2RYUnZjMmhvT21KWGJFVkJhV1JKTUhGQ2RVbzVlbXhKU3oxVWQyTjZjVFIzVUhwdlRWUXIifX19
kind: Secret
metadata:
  creationTimestamp: null
  name: acrsec
type: kubernetes.io/dockerconfigjson
fire@ashutoshhs-MacBook-Air ~ % kubectl  create  secret  docker-registry   acrsec  --docker-server=ashutoshh.azurecr.io       --docker-username=ashutoshh  --docker-password="bWlEAidI0qBuJ9zlIK=Twczq4wPzoMT+"

```

### secret info 

<img src="secretinfo.png">


