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

### More pod details 

```
kubectl  get  po
NAME          READY   STATUS    RESTARTS   AGE
ashupod-123   1/1     Running   0          49m
ashutoshh@Azure:~$ kubectl  get  po  -o wide
NAME          READY   STATUS    RESTARTS   AGE   IP           NODE                                NOMINATED NODE   READINESS GATES
ashupod-123   1/1     Running   0          49m   10.244.0.5   aks-agentpool-40604622-vmss000002   <none>           <none>
ashutoshh@Azure:~$

```

### Describe pod info 

```
 kubectl  describe pod ashupod-123
Name:         ashupod-123
Namespace:    default
Priority:     0
Node:         aks-agentpool-40604622-vmss000002/10.240.0.6
Start Time:   Tue, 08 Feb 2022 05:33:36 +0000
Labels:       <none>
Annotations:  cni.projectcalico.org/containerID: 7015e621ca487dd0c6746c6d72cd6fe818b65ab1cf3cdc3e1cdbf6f52b071c8e
              cni.projectcalico.org/podIP: 10.244.0.5/32
              cni.projectcalico.org/podIPs: 10.244.0.5/32
Status:       Running
IP:           10.244.0.5
IPs:
  IP:  10.244.0.5
Containers:
  ashuc1:
    Container ID:   containerd://e55c25eb128494d22fcc25d87c1193f9aa20b2dc5cd9cd105663a1f266306a2f
    Image:          docker.io/dockerashu/ashuwebapp:pwcfeb82022
    Image ID:       docker.io/dockerashu/ashuwebapp@sha256:a89ffb268e2ce632c29508ec8ffcbb4dd27ea982ec03c63e8facd53f7e53fc83
    Port:           80/TCP
```

### accessing container inside POD 

```
 kubectl exec -it  ashupod-123 --  bash
root@ashupod-123:/#
root@ashupod-123:/#
root@ashupod-123:/# uname -r
5.4.0-1067-azure
root@ashupod-123:/# cat /etc/os-release
PRETTY_NAME="Debian GNU/Linux 11 (bullseye)"
NAME="Debian GNU/Linux"
VERSION_ID="11"
VERSION="11 (bullseye)"
VERSION_CODENAME=bullseye
ID=debian
HOME_URL="https://www.debian.org/"
SUPPORT_URL="https://www.debian.org/support"
BUG_REPORT_URL="https://bugs.debian.org/"
root@ashupod-123:/# ls
bin   dev                  docker-entrypoint.sh  home  lib64  mnt  proc  run   srv  tmp  var
boot  docker-entrypoint.d  etc                   lib   media  opt  root  sbin  sys  usr
root@ashupod-123:/# exit
exit

```

### az login --

```
 az login 
The default web browser has been opened at https://login.microsoftonline.com/organizations/oauth2/v2.0/authorize. Please continue the login in the web browser. If no web browser is available or if the web browser fails to open, use device code flow with `az login --use-device-code`.
[
  {
    "cloudName": "AzureCloud",
    "homeTenantId": "199ee0d2-f16c-4bcc-8282-a04937c02e5d",
    "id": "a91b42ca-7e1b-41d2-89d1-be09970ee26c",
    "isDefault": true,
    "managedByTenants": [],
    "name": "Azure Pass - Sponsorship",
    "state": "Enabled",
    "tenantId": "199ee0d2-f16c-4bcc-8282-a04937c02e5d",
    "user": {
      "name": "learntechbyme@gmail.com",
      "type": "user"
    }
  }
]

```
### az login from local and setup cred 

```
fire@ashutoshhs-MacBook-Air ~ % az account set --subscription a91b42ca-7e1b-41d2-89d1-be09970ee26c
fire@ashutoshhs-MacBook-Air ~ % az aks get-credentials --resource-group aks_training --name 123
/Users/fire/.kube/config has permissions "644".
It should be readable and writable only by its owner.
Merged "123" as current context in /Users/fire/.kube/config
fire@ashutoshhs-MacBook-Air ~ % 
fire@ashutoshhs-MacBook-Air ~ % kubectl  get  nodes
NAME                                STATUS   ROLES   AGE     VERSION
aks-agentpool-40604622-vmss000000   Ready    agent   4h41m   v1.21.7
aks-agentpool-40604622-vmss000001   Ready    agent   4h41m   v1.21.7
aks-agentpool-40604622-vmss000002   Ready    agent   4h41m   v1.21.7
fire@ashutoshhs-MacBook-Air ~ % kubectl get  po
NAME          READY   STATUS    RESTARTS   AGE
ashupod-123   1/1     Running   0          62m

```

### COnnecting AKS from local system -- using azure cli 

[Download](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli)


### Deleting pods 
```
 kubectl get  pods
NAME             READY   STATUS    RESTARTS   AGE
ashupod-123      1/1     Running   0          76m
ashupod-123444   1/1     Running   0          6m31s
fire@ashutoshhs-MacBook-Air yamls % kubectl get  pods -o wide
NAME             READY   STATUS    RESTARTS   AGE     IP           NODE                                NOMINATED NODE   READINESS GATES
ashupod-123      1/1     Running   0          76m     10.244.0.5   aks-agentpool-40604622-vmss000002   <none>           <none>
ashupod-123444   1/1     Running   0          6m36s   10.244.0.7   aks-agentpool-40604622-vmss000002   <none>           <none>
fire@ashutoshhs-MacBook-Air yamls % kubectl  delete  pod  --all
pod "ashupod-123" deleted
pod "ashupod-123444" deleted


```
### namespaces ---

<img src="ns.png">

###

```
amls % 
fire@ashutoshhs-MacBook-Air yamls % kubectl  get  namespaces
NAME              STATUS   AGE
calico-system     Active   5h4m
default           Active   5h6m
kube-node-lease   Active   5h6m
kube-public       Active   5h6m
kube-system       Active   5h6m
tigera-operator   Active   5h5m
fire@ashutoshhs-MacBook-Air yamls % 
fire@ashutoshhs-MacBook-Air yamls % kubectl  get  pods
No resources found in default namespace.
fire@ashutoshhs-MacBook-Air yamls % 
fire@ashutoshhs-MacBook-Air yamls % kubectl  get  ns  
NAME              STATUS   AGE
calico-system     Active   5h5m
default           Active   5h7m
kube-node-lease   Active   5h7m
kube-public       Active   5h7m
kube-system       Active   5h7m
tigera-operator   Active   5h7m
```

### kube-system --

```
 kubectl  get  po -n kube-system 
NAME                                  READY   STATUS    RESTARTS   AGE
coredns-845757d86-jkjv9               1/1     Running   0          5h8m
coredns-845757d86-jxrh7               1/1     Running   0          5h6m
coredns-autoscaler-5f85dc856b-qgkjx   1/1     Running   0          5h8m
csi-azuredisk-node-7bxfq              3/3     Running   0          5h7m
csi-azuredisk-node-956kb              3/3     Running   0          5h6m
csi-azuredisk-node-dfhqk              3/3     Running   0          5h6m
csi-azurefile-node-fx7wf              3/3     Running   0          5h6
```

### creating custom namespaces --

```
1009  kubectl  create   namespace  ashu-webappsonly 
 1010  kubectl  get   ns
 1011  kubectl  create   namespace  ashu-databases
 1012  kubectl  get   ns
fire@ashutoshhs-MacBook-Air yamls % kubectl get  ns
NAME               STATUS   AGE
ashu-databases     Active   23s
ashu-webappsonly   Active   46s
calico-system      Active   5h10m
default            Active   5h11m

```

### namespace based pod deployment 

```
 kubectl  apply -f  pod1.yaml 
pod/ashupod-123444 created
fire@ashutoshhs-MacBook-Air yamls % kubectl get  po
No resources found in default namespace.
fire@ashutoshhs-MacBook-Air yamls % kubectl get  po -n  ashu-webappsonly 
NAME             READY   STATUS    RESTARTS   AGE
ashupod-123444   1/1     Running   0          17s
```

### AUtogenerate yaml / json 

```
kubectl  run  ashupod1  --image=alpine  --command ping localhost --dry-run=client -o yaml     >autopod.yaml
```
### deploy / delete 

```
% kubectl apply -f autopod.yaml 
pod/ashupod1 created
fire@ashutoshhs-MacBook-Air yamls % kubectl  get  po
NAME       READY   STATUS    RESTARTS   AGE
ashupod1   1/1     Running   0          3s
fire@ashutoshhs-MacBook-Air yamls % kubectl delete  -f autopod.yaml
pod "ashupod1" deleted


```
### settting default namespace in client side system 

```
 kubectl  config  set-context  --current --namespace=ashu-webappsonly
```

### webapp deployment in aks using pod under custom namespaces --

```
 kubectl  run  ashuwebapp --image=dockerashu/ashuwebapp:pwcfeb82022  --port=80   --dry-run=client  -o yaml  
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: ashuwebapp
  name: ashuwebapp
spec:
  containers:
  - image: dockerashu/ashuwebapp:pwcfeb82022
    name: ashuwebapp
    ports:
    - containerPort: 80
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
fire@ashutoshhs-MacBook-Air yamls % kubectl  run  ashuwebapp --image=dockerashu/ashuwebapp:pwcfeb82022  --port=80   --dry-run=client  -o yaml   >mywebapp.yaml

```

### deploy and check 

```
% ls
autopod.yaml    check.yaml      mywebapp.yaml   pod1.yaml
fire@ashutoshhs-MacBook-Air yamls % kubectl apply -f  mywebapp.yaml 
pod/ashuwebapp created
fire@ashutoshhs-MacBook-Air yamls % kubectl  get  pods
NAME         READY   STATUS    RESTARTS   AGE
ashuwebapp   1/1     Running   0          6s
fire@ashutoshhs-MacBook-Air yamls % kubectl  get  pods -o wide
NAME         READY   STATUS    RESTARTS   AGE   IP            NODE                                NOMINATED NODE   READINESS GATES
ashuwebapp   1/1     Running   0          11s   10.244.0.11   aks-agentpool-40604622-vmss000002   <none>           <none>

```

## Networking in k8s

<img src="k8snet.png">

## Networking for containers 

<img src="cnet.png">

### list of k8s CNI plugins 

<img src="cni.png">

###  CNI bridge based communicaiton 

```
 1004  kubectl  run pod1  --image=alpine --command ping localhost 
 1005  kubectl  run pod2  --image=alpine --command ping localhost 
 1006  kubectl  get  po -o wide
 1007  kubectl  get  po -o wide
 1008  kubectl  exec -it pod1  -- sh 
 1009  history
 1010  kubectl  run pod3 --namespace=default   --image=alpine --command ping localhost 
 1011  kubectl  get  po -o wide
 1012  kubectl  get  po -o wide -n default
 1013  kubectl  exec -it pod1  -- sh 


```

### from kubectl client machine Minion node tunnel 

```
kubectl  port-forward  ashuwebapp  1234:80 
Forwarding from 127.0.0.1:1234 -> 80
Forwarding from [::1]:1234 -> 80
Handling connection for 1234
Handling connection for 1234
Handling connection for 1234
Handling connection for 1234
Handling connection for 1234

```

### any end user access app running in the POD --

### network flow 

### Azure Internal LB (indepent of k8s)

<img src="azlb.png">

### Internal LB concept 

<img src="lb.png">

## Intro to service --

<img src="svc.png">

### service type 

<img src="svctype.png">

### creating service 

```
 kubectl  get  pods         
NAME         READY   STATUS    RESTARTS   AGE
ashuwebapp   1/1     Running   0          47m
fire@ashutoshhs-MacBook-Air yamls % 
fire@ashutoshhs-MacBook-Air yamls % 
fire@ashutoshhs-MacBook-Air yamls % kubectl  get  service 
No resources found in ashu-webappsonly namespace.
fire@ashutoshhs-MacBook-Air yamls % kubectl  create  service  
Create a service using a specified subcommand.

Aliases:
service, svc

Available Commands:
  clusterip    Create a ClusterIP service
  externalname Create an ExternalName service
  loadbalancer Create a LoadBalancer service
  nodeport     Create a NodePort service

```
### creating service 

```
kubectl  expose  pod  ashuwebapp  --type LoadBalancer  --port 1234  --target-port 80 --name   ashusvc1   --dry-run=client -o yaml 
apiVersion: v1
kind: Service
metadata:
  creationTimestamp: null
  labels:
    run: ashuwebapp
  name: ashusvc1
spec:
  ports:
  - port: 1234
    protocol: TCP
    targetPort: 80
  selector:
    run: ashuwebapp
  type: LoadBalancer
status:
  loadBalancer: {}
```

### Deploy svc 

```
 % kubectl  apply -f  myappsvc.yaml 
service/ashusvc1 created
fire@ashutoshhs-MacBook-Air yamls % kubectl  get  service 
NAME       TYPE           CLUSTER-IP   EXTERNAL-IP   PORT(S)          AGE
ashusvc1   LoadBalancer   10.0.47.10   <pending>     1234:32395/TCP   8s
fire@ashutoshhs-MacBook-Air yamls % kubectl  get  svc     
NAME       TYPE           CLUSTER-IP   EXTERNAL-IP   PORT(S)          AGE
ashusvc1   LoadBalancer   10.0.47.10   <pending>     1234:32395/TCP   12s
fire@ashutoshhs-MacBook-Air yamls % kubectl  get  svc
NAME       TYPE           CLUSTER-IP   EXTERNAL-IP   PORT(S)          AGE
ashusvc1   LoadBalancer   10.0.47.10   13.71.55.49   1234:32395/TCP   115s

```

## Storage in k8s 

<img src="st.png">

### k8s volume section 

<img src="volume.png">

### generating pod with random data generation 

```
 kubectl  run  randomdata   --image=alpine  --dry-run=client -o yaml  >randomdata.yaml 
```

### deploy pod 

<img src="podep.png">

### checking container data 

```
kubectl get po
NAME         READY   STATUS    RESTARTS   AGE
randomdata   1/1     Running   0          90s
fire@ashutoshhs-MacBook-Air yamls % kubectl  exec -it  randomdata  -- sh 
/ # cat /etc/os-release 
NAME="Alpine Linux"
ID=alpine
VERSION_ID=3.15.0
PRETTY_NAME="Alpine Linux v3.15"
HOME_URL="https://alpinelinux.org/"
BUG_REPORT_URL="https://bugs.alpinelinux.org/"
/ # 
/ # cd  /mnt/
/mnt # ls
time.txt
/mnt # cat  time.txt 
Tue Feb  8 11:24:25 UTC 2022
Tue Feb  8 11:24:35 UTC 2022
Tue Feb  8 11:24:45 UTC 2022
Tue Feb  8 11:24:55 UTC 2022
Tue Feb  8 11:25:05 UTC 2022
Tue Feb  8 11:25:15 UTC 2022
Tue Feb  8 11:25:25 UTC 2022
Tue Feb  8 11:25:35 UTC 2022
Tue Feb  8 11:25:45 UTC 2022
Tue Feb  8 11:25:55 UTC 2022
Tue Feb  8 11:26:05 UTC 2022

```

### data damaged 

```
% kubectl delete  pod randomdata
pod "randomdata" deleted
fire@ashutoshhs-MacBook-Air yamls % kubectl  get  po              
No resources found in ashu-webappsonly namespace.
fire@ashutoshhs-MacBook-Air yamls % kubectl apply -f randomdata.yaml 
pod/randomdata created
fire@ashutoshhs-MacBook-Air yamls % kubectl  exec -it  randomdata  -- sh 
/ # cd /mnt/
/mnt # ls
time.txt
/mnt # wc  -l time.txt 
2 time.txt
/mnt # exit

```

### using k8s cluster based local storage 

<img src="localst.png">

### volume sources and pod 

<img src="volsrc.png">

## PV and PVC 

<img src="pv.png">



