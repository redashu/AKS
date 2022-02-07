# AKS

## app deployment / testing problem in Past : 

<img src="prob.png">

### bare-metal problem solved by Hypervisor 

<img src="hyper.png">

### VM proble 

<img src="vm1.png">

### OS components 

<img src="os.png">

### vm vs cre 

<img src="cre.png">

### app containerization process -- BUild -- RUN -- 

<img src="cont1.png">

### image building process 

<img src="process.png">

### image build done 

```
docker build -t ashuwebapp:v1 . 

```

### creating container --

```
docker  run  -d  --name  ashuc1  -p  1234:80  ashuwebapp:v1  
90b6d9c8914c56b10bc12bb349536ed0c9ab2717fb82359974f2c439442bf363
[test@ip-172-31-84-215 data]$ 
[test@ip-172-31-84-215 data]$ 
[test@ip-172-31-84-215 data]$ docker  ps
CONTAINER ID   IMAGE           COMMAND                  CREATED          STATUS          PORTS                                   NAMES
90b6d9c8914c   ashuwebapp:v1   "/docker-entrypoint.…"   3 seconds ago    Up 2 seconds    0.0.0.0:1234->80/tcp, :::1234->80/tcp   ashuc1
d9c295f968d6   ponwebapp:v1    "/docker-entrypoint.…"   34 minutes ago   Up 34 minutes   0.0.0.0:80->80/tcp, :::80->80/tcp       elated_chandrasekhar

```

### getting started with AKS 

<img src="aks.png">

### Container runtime Engine problems --

<img src="creprob.png">

### k8s 

<img src="k8s.png">

### intro 

<img src="k8sintro.png">

### hardware arch 

<img src="infra.png">

### k8s Minion Side --

<img src="min1.png">

### Kube-apiserver 

<img src="apiserver.png">

### k8s client side software 

<img src="k8scli.png">

### checking --

```
kubectl  version --client 
Client Version: version.Info{Major:"1", Minor:"22

```

### kube-schedular --

<img src="ksch.png">

### etcd 

<img src="etcd.png">

### manage vs unmanaged k8s cluster

<img src="k8setup.png">

### azure k8s access

<img src="aks.png">

### any app deployment process in aks 

<img src="akss.png">

### from container image to running container journey 

<img src="cont22.png">


