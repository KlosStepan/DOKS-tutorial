# Install MySQL
There are several steps that have to be performed.  
## Create Secret for MySQL ROOT_PASSWORD
1. Encode master password via script to put into **Secret**
2. Execute `kubectl apply -f 0-mysql-secret.yaml`
```
secret/mysqldb-secrets created
```
3. `kubectl get secret`
```
NAME                  TYPE                                  DATA   AGE
default-token-4nxzv   kubernetes.io/service-account-token   3      97d
mysqldb-secrets       Opaque                                1      9m13s
pwnstepo-registry     kubernetes.io/dockerconfigjson        1      96d
```
4. `kubectl describe secret mysqldb-secrets`
```
Name:         mysqldb-secrets
Namespace:    default
Labels:       <none>
Annotations:  <none>

Type:  Opaque

Data
====
ROOT_PASSWORD:  11 bytes
```
## Create PV and PVC on it. Create MySQL Deployment using PVC as storage.
1. Execute `kubectl apply -f 1-pv-pvc-mysql.yaml`
```
persistentvolume/mysql-pv created
persistentvolumeclaim/mysql-pvc-claim created
```
2. Execute `kubectl apply -f 2-deployment-mysql.yaml`
```
service/mysql created
```
## Expose MySQL as a Service.
1. Execute `3-service-mysql.yaml`
```
service/mysql-service created
```
2. Execute `kubectl get svc` to confirm that service is created successfully.
```
NAME                      TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)    AGE
hello-kubernetes-first    ClusterIP   10.245.42.39    <none>        80/TCP     36d
hello-kubernetes-second   ClusterIP   10.245.195.40   <none>        80/TCP     36d
kubernetes                ClusterIP   10.245.0.1      <none>        443/TCP    97d
lnmap                     ClusterIP   10.245.63.123   <none>        80/TCP     30d
mysql                     ClusterIP   None            <none>        3306/TCP   7m1s
mysql-service             ClusterIP   10.245.2.59     <none>        3306/TCP   28s
ubyvapuda-cz              ClusterIP   10.245.85.192   <none>        80/TCP     36d
vipsidla-cz               ClusterIP   10.245.15.78    <none>        80/TCP     29d
```
