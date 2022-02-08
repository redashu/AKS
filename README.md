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

### smallest unit in k8s is POD 

<img src="pod.png">

### Deploy my first POD 

```
ashutoshh@Azure:~$ whoami
ashutoshh
ashutoshh@Azure:~$ pwd
/home/ashutoshh
ashutoshh@Azure:~$ mkdir  ashu_yamls
ashutoshh@Azure:~$ cd ashu_yamls/
ashutoshh@Azure:~/ashu_yamls$
ashutoshh@Azure:~/ashu_yamls$ vim  ashupod1.yaml
ashutoshh@Azure:~/ashu_yamls$ vim  ashupod1.yaml
ashutoshh@Azure:~/ashu_yamls$ cat ashupod1.yaml
apiVersion: v1
kind: Pod
metadata:
 name: ashupod-123  # name of pod
spec: # appinfo like container / storage / schedular
 containers:
 - image: docker.io/dockerashu/ashuwebapp:pwcfeb82022
   name: ashuc1 # you can use same container name
   ports: # optional part these days
   - containerPort: 80
ashutoshh@Azure:~/ashu_yamls$ kubectl  apply -f ashupod1.yaml
pod/ashupod-123 created
ashutoshh@Azure:~/ashu_yamls$ kubectl  get pods
NAME          READY   STATUS              RESTARTS   AGE
ashupod-123   0/1     ContainerCreating   0          15s
ashutoshh@Azure:~/ashu_yamls$ kubectl  get pods
NAME          READY   STATUS    RESTARTS   AGE
ashupod-123   1/1     Running   0          34s

```

