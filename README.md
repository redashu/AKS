# AKS

### creating and setting namespaces 

```

fire@ashutoshhs-MacBook-Air ~ % kubectl  create  namespace  ashu-space  --dry-run=client -o yaml 
apiVersion: v1
kind: Namespace
metadata:
  creationTimestamp: null
  name: ashu-space
spec: {}
status: {}
fire@ashutoshhs-MacBook-Air ~ % kubectl  create  namespace  ashu-space                           
namespace/ashu-space created
fire@ashutoshhs-MacBook-Air ~ % kubectl get  ns
NAME              STATUS   AGE
ashu-space        Active   3s
calico-system     Active   60m
default           Active   62m
kube-node-lease   Active   63m
kube-public       Active   63m
kube-system       Active   63m
tigera-operator   Active   62m
fire@ashutoshhs-MacBook-Air ~ % kubectl config set-context  --current --namespace=ashu-space
Context "nonprod" modified.
fire@ashutoshhs-MacBook-Air ~ % 
fire@ashutoshhs-MacBook-Air ~ % kubectl get  po 
No resources found in ashu-space namespace.

```
## k8s controller 

<img src="cont.png">

### journey from Pod -RC - RS - deployment 

<img src="dep.png">

### scaling story of POD 

<img src="scale.png">

### create deployment --

```
kubectl  create  deployment ashudep  --image=dockerashu/ashuwebapp:pwcfeb82022  --port 80 --dry-run=client -o yaml

```

### deploy the deployment 

```
kubectl  apply -f  deployment.yaml 
deployment.apps/ashudep created
fire@ashutoshhs-MacBook-Air yamls % kubectl get  deployment 
NAME      READY   UP-TO-DATE   AVAILABLE   AGE
ashudep   1/1     1            1           10s
fire@ashutoshhs-MacBook-Air yamls % kubectl get  deploy     
NAME      READY   UP-TO-DATE   AVAILABLE   AGE
ashudep   1/1     1            1           13s
 kubectl get  po    
NAME                       READY   STATUS    RESTARTS   AGE
ashudep-77b65b6d94-4szl9   1/1     Running   0          54s
fire@ashutoshhs-MacBook-Air yamls % kubectl get  po -o wide
NAME                       READY   STATUS    RESTARTS   AGE   IP            NODE                                NOMINATED NODE   READINESS GATES
ashudep-77b65b6d94-4szl9   1/1     Running   0          62s   10.244.2.11   aks-agentpool-25714751-vmss000001   <none>           <none>
fire@ashutoshhs-MacBook-Air yamls % 


```

### manual scaling --

```
 kubectl  get deployment 
NAME      READY   UP-TO-DATE   AVAILABLE   AGE
ashudep   1/1     1            1           6m39s
fire@ashutoshhs-MacBook-Air ~ % 
fire@ashutoshhs-MacBook-Air ~ % 
fire@ashutoshhs-MacBook-Air ~ % kubectl  scale  deployment  ashudep  --replicas=3
deployment.apps/ashudep scaled
fire@ashutoshhs-MacBook-Air ~ % 
fire@ashutoshhs-MacBook-Air ~ % kubectl  get deployment                          
NAME      READY   UP-TO-DATE   AVAILABLE   AGE
ashudep   1/3     3            1           7m4s
fire@ashutoshhs-MacBook-Air ~ % kubectl  get deployment 
NAME      READY   UP-TO-DATE   AVAILABLE   AGE
ashudep   3/3     3            3           7m9s
fire@ashutoshhs-MacBook-Air ~ % kubectl  get po         
NAME                       READY   STATUS    RESTARTS   AGE
ashudep-77b65b6d94-hdb84   1/1     Running   0          15s
ashudep-77b65b6d94-jnggb   1/1     Running   0          102s
ashudep-77b65b6d94-rsnmw   1/1     Running   0          15s
fire@ashutoshhs-MacBook-Air ~ % kubectl  get po -o wide
NAME                       READY   STATUS    RESTARTS   AGE    IP            NODE                                NOMINATED NODE   READINESS GATES
ashudep-77b65b6d94-hdb84   1/1     Running   0          25s    10.244.0.5    aks-agentpool-25714751-vmss000000   <none>           <none>
ashudep-77b65b6d94-jnggb   1/1     Running   0          112s   10.244.2.13   aks-agentpool-25714751-vmss000001   <none>           <none>
ashudep-77b65b6d94-rsnmw   1/1     Running   0          25s    10.244.1.7    aks-agentpool-25714751-vmss000002   <none>           <none>

```



