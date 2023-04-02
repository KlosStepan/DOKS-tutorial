# DigitalOcean Kubernetes production Cluster
Description of DOKS setup and tools/applications w/ its configurations.  

**!!! All stuff is my own configs/notes, I'm not 100% sure that everything is correct&safe.**
## Setup Overview 
We use DigitalOcean Managed Kubernetes in Amsterdam (Datacenter 3/AMS3). The setup is following:
- Kubernetes Cluster w/ **2 Node** workers **s-1vcpu-2gb-intel**, 
- **DigitalOcean Load Balancer** for accepting traffic - incoming HTTP requests,
- **Ingress Controller** w/ nginx in for routing traffic within Cluster into Kubernetes Services.  

We use [**Kubernetes Dashboard**](https://github.com/kubernetes/dashboard), [**Lens**](https://k8slens.dev/desktop.html) (via kubectl) and [**kubectl**](https://kubernetes.io/docs/tasks/tools/) for monitoring and adminsitration. Majority of application administration is performed by applying config files in following manner 
`kubectl apply -f kubernetes-ingress-config.yaml`. Overview is carried on either in GUI or by querying via **kubectl**, such as `kubectl get nodes`, `kubectl get deployments` or `kubectl describe ingress hello-kubernetes-ingress`.
## Cluster Visialization
1. <u>Cluster Visualization</u> - blue components are either external (DNS Services, Load Balancer / DBs or Image Registry) or added to the cloud later (Ingress). It shows architecture of **MySQL** & **Redis** databases in cluster and one PHP app utilizing it. Orange color is used for external communication/traffic.
2. <u>Cluster Running architecture</u> - 1 DOKS (Master) & 2 Nodes (2nd+ nodes are optional - therefore blue).
3. <u>DOKS Volume policy</u> - 1 Volume can be attached to just 1 Node at the time. We show how to run 1/1 Replica databasesm, which is sufficient for simple usecase like ours. You can use Managed by cloud provider ones. 
4. <u>YAML config files example schema</u>.
![DOKS Cluster design](/misc/kubernetes-design-1.png "doks-cluster-design")
## Tools running in Kubernetes Cluster
- **Ingress** - (1-click-app/heml) **kubernetes-ingress-config.yaml**,
- ~~**cert-manager** - (1-click-app/heml) !TODO configs,~~
- **mysql-deployment/** - MySQL Database in Kubernetes Cluster.
    - **0-encode-base64.sh** - Prepare base64 encoded root passwd. 
    - **0-mysql-secret.yaml** - Apply MySQL secret onto cluster.
    - **1-pv-pvc-mysql.yaml** - Create PV and get PVC (**Persistent Volume** and subsequent **Persistent Volume Claim** on it).
    - **2-deployment-mysql.yaml** - Deployment of MySQL utilising PVC as volume and mounted **/var/lib/mysql** on it.
    - **3-service-mysql.yaml** - Expose as a Service **mysql-service** port 3306.
- **adminer** - (adminer Docker image) Adminer GUI client for browsing DBs running on **mysql-service** (expose from mysql-deployment pt.3) run at **adminer.yourdomain.com**,    
- **redis-deployment/** **Re**mote **Di**ctionary **S**erver in Kubernetes Cluster.  
    - **0-encode-base64.zsh** - Prepare base64 encoded root passwd. 
    - **0-redis-secret.yaml** - Apply Redis secret onto cluster.
    - **1-pv-pvc-redis.yaml** - Create PV and get PVC (**Persistent Volume** and subsequent **Persistent Volume Claim** on it).
    - **2-deployment-redis.yaml** - Deployment of Redis utilising PVC as volume and mounted **/var/lib/mysql** on it.
    - **3-service-redis.yaml** - Expose as a Service **redis-service** port 6379.

!IN THE MAKING -  Prometheus monitoring - [DOKS Helm package bugged](https://marketplace.digitalocean.com/apps/kubernetes-monitoring-stack).
## Detailed installation of Database README.md tutorial in:
- mysql-deployment/README.md,
- redis-deployment/README.md.
# Configs for apps to be run in Kubernetes Cluster
- **adminer.yaml** - Adminer tool from up. //<- use this
- **app-hello-kubernetes-first.yaml** - Dummy application 1. //<- use this
- **app-hello-kubernetes-second.yaml** - Dummy application 2. //<- use this
- ~~**app-swimmpair.yaml** - **Production**~~ SwimmPair - example of LAMP web application w/ all necessary services. //<- don't use this




