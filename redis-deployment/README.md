# Install Redis to K8s Cluster
Follow these 3 steps to achieve desired functionality (like previously shown `mysql-deployment`).  
https://hub.docker.com/_/redis

## 0. Generate base64 password & apply Kubernetes Secret
Generate base64 password and place it into redis-secret file.
```zsh
➜  redis-deployment git:(main) ✗ chmod u+x 0-encode-base64.zsh 
➜  redis-deployment git:(main) ✗ zsh 0-encode-base64.zsh password 
cGFzc3dvcmQ=
➜  redis-deployment git:(main) ✗ kubectl apply -f 0-redis-secret.yaml 
secret/redis-secrets created
```
## 1. Create PV & and make PVC on it
```zsh
➜  redis-deployment git:(main) ✗ kubectl apply -f 1-pv-pvc-redis.yaml  
```
<p align="center">
  <img src="misc/redis-1.png" alt="Redis PV & PVC running"/>
</p>

## 2. Create Service (internal) & Deployment  
```zsh
➜  redis-deployment git:(main) ✗ kubectl apply -f 2-deployment-redis.yaml 
```
<p align="center">
  <img src="misc/redis-2.png" alt="Redis Service(internal) & Deployment running"/>
</p>

## 3. Create Service (public) - link the internal one
```zsh
➜  redis-deployment git:(main) ✗ kubectl apply -f 3-service-redis.yaml   
```
<p align="center">
  <img src="misc/redis-3.png" alt="Redis Public Service running"/>
</p>

___
## Where/how to use Redis
Redis should be running on `tcp://redis-service:6379?auth=PASSWD`  
Both **MySQL** and **Redis** are used by multireplica deployment of SwimmPair.  
<p align="center">
  <img src="misc/K8s_SwimmPair.png" alt="MySQL Service + Redis Service"/>
</p>
They run at usable for other applications:

- mysql-service:3306,
- redis-service:6379.
