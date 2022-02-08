# AKS

## day1 Revision --

<img src="rev.png">

### connecting to aks using azure shell 

<img src="azshell.png">

### kubectl cred reading from client machine 

<img src="readcred.png">

### kubectl some commands 

```
 kubectl  get  nodes
NAME                                STATUS   ROLES   AGE    VERSION
aks-agentpool-40604622-vmss000000   Ready    agent   159m   v1.21.7
aks-agentpool-40604622-vmss000001   Ready    agent   159m   v1.21.7
aks-agentpool-40604622-vmss000002   Ready    agent   159m   v1.21.7
ashutoshh@Azure:~$
ashutoshh@Azure:~$ kubectl  get  no
NAME                                STATUS   ROLES   AGE    VERSION
aks-agentpool-40604622-vmss000000   Ready    agent   160m   v1.21.7
aks-agentpool-40604622-vmss000001   Ready    agent   160m   v1.21.7
aks-agentpool-40604622-vmss000002   Ready    agent   160m   v1.21.7
ashutoshh@Azure:~$ kubectl  get  no -o wide
NAME                                STATUS   ROLES   AGE    VERSION   INTERNAL-IP   EXTERNAL-IP   OS-IMAGE             KERNEL-VERSION     CONTAINER-RUNTIME
aks-agentpool-40604622-vmss000000   Ready    agent   160m   v1.21.7   10.240.0.4    <none>        Ubuntu 18.04.6 LTS   5.4.0-1067-azure   containerd://1.4.9+a
zure
aks-agentpool-40604622-vmss000001   Ready    agent   160m   v1.21.7   10.240.0.5    <none>        Ubuntu 18.04.6 LTS   5.4.0-1067-azure   containerd://1.4.9+a
zure
aks-agentpool-40604622-vmss000002   Ready    agent   160m   v1.21.7   10.240.0.6    <none>        Ubuntu 18.04.6 LTS   5.4.0-1067-azure   containerd://1.4.9+azure

```
### pushing image to docker hub first 

<img src="imgpush.png">

### pushin image 

```
docker  tag  ashuwebapp:v1   docker.io/dockerashu/ashuwebapp:pwcfeb82022

 docker  login 
Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.
Username: dockerashu
Password: 
WARNING! Your password will be stored unencrypted in /home/user1/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded
[user1@ip-172-31-84-215 ~]$ docker  push docker.io/dockerashu/ashuwebapp:pwcfeb82022
The push refers to repository [docker.io/dockerashu/ashuwebapp]
fe24c9473bac: Pushed 
762b147902c0: Pushed 
235e04e3592a: Pushed 
6173b6fa63db: Pushed 
9a94c4a55fe4: Pushed 
9a3a6af98e18: Pushed 
7d0ebbe3f5d2: Pushed 
pwcfeb82022: digest: sha256:a89ffb268e2ce632c29508ec8ffcbb4dd27ea982ec03c63e8facd53f7e53fc83 size: 1780
[user1@ip-172-31-84-215 ~]$ docker logout
Removing login credentials for https://index.docker.io/v1/

```
