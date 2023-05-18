# DigitalOcean Kubernetes production Cluster
Description of our DOKS setup and tools/applications w/ configurations and brief tutorials.  

**-> All the stuff comes without warranty! Think twice about any configuration or approach here!**
## Setup Overview 
We use DigitalOcean Managed Kubernetes (in Amsterdam Datacenter 3/AMS3).  
The setup is following:
- Kubernetes Cluster w/ **2 Node** workers **s-1vcpu-2gb-intel**, 
- **DigitalOcean Load Balancer** for accepting traffic - incoming HTTP requests,
- **Ingress Controller** w/ nginx for routing traffic within Cluster into Kubernetes Services.  

We use [**Kubernetes Dashboard**](https://github.com/kubernetes/dashboard), [**Lens**](https://k8slens.dev/desktop.html) (via kubectl) and [**kubectl**](https://kubernetes.io/docs/tasks/tools/) for monitoring and adminsitration. Majority of application administration is performed by applying config files in following manner 
`kubectl apply -f kubernetes-ingress-config.yaml`. Overview is carried on either in GUI or by querying via **kubectl**, such as `kubectl get nodes`, `kubectl get deployments` or `kubectl describe ingress hello-kubernetes-ingress`.  
___  

## Our K8s cluster overview
1. <u>Cluster Visualization</u> - blue components are either external (DNS Services, Load Balancer / DBs or Image Registry) or added to the cluster later (Ingress). It shows architecture of **MySQL** & **Redis** databases in cluster and one PHP app utilizing it. Orange color is used for external communication/traffic.
2. <u>Running cluster architecture</u> - 1 DOKS (Master) & 2 Nodes (2nd+ nodes are optional - therefore blue).
3. <u>DOKS Volume policy</u> - 1 Volume can be attached to just 1 Node at the time. We show how to run 1/1 Replica databases, which is sufficient for simple usecase like ours. You can use Managed by cloud provider ones. 
4. <u>YAML config file - basic example schema</u>.
![DOKS Cluster design](/misc/kubernetes-design-1.png "doks-cluster-design")
## Tools running in K8s cluster
- **Ingress** - (1-Click apps/Helm) **kubernetes-ingress-config.yaml**,
- ~~**cert-manager** - (1-Click apps/Helm) !TODO configs,~~
- **mysql-deployment/** - MySQL Database Server in K8s Cluster.
    - **0-encode-base64.sh** - Prepare base64 encoded root passwd. 
    - **0-mysql-secret.yaml** - Apply MySQL Secret onto cluster.
    - **1-pv-pvc-mysql.yaml** - Create PV and get PVC (**Persistent Volume** and subsequent **Persistent Volume Claim** on it).
    - **2-deployment-mysql.yaml** - Deployment of MySQL utilising PVC as volume and mounted **/var/lib/mysql** on it.
    - **3-service-mysql.yaml** - Expose as **mysql-service** port 3306.
- **Adminer** - (adminer Docker image Deployment) Adminer GUI client for browsing DBs running on **mysql-service** (expose from mysql-deployment pt.3) run at **adminer.yourdomain.com**,    
- **redis-deployment/** **Re**mote **Di**ctionary **S**erver in K8s Cluster.  
    - **0-encode-base64.zsh** - Prepare base64 encoded root passwd. 
    - **0-redis-secret.yaml** - Apply Redis Secret onto cluster.
    - **1-pv-pvc-redis.yaml** - Create PV and get PVC (**Persistent Volume** and subsequent **Persistent Volume Claim** on it).
    - **2-deployment-redis.yaml** - Deployment of Redis utilising PVC as volume and mounted **/var/lib/mysql** on it.
    - **3-service-redis.yaml** - Expose as **redis-service** port 6379.
 - ~~**Prometheus monitoring**~~ - IN THE PROCESS. 1-Click app [DOKS Helm package bugged](https://marketplace.digitalocean.com/apps/kubernetes-monitoring-stack).

## Detailed installation of Databases - steps in READMEs:
- [mysql-deployment/README.md](https://github.com/KlosStepan/DOKS-tutorial/tree/main/mysql-deployment),
- [redis-deployment/README.md](https://github.com/KlosStepan/DOKS-tutorial/tree/main/redis-deployment).

# App Configs - run in K8s cluster
- **adminer.yaml** - Adminer tool from up. // <- use this
- **app-hello-kubernetes-first.yaml** - Dummy application 1. // <- use this
- **app-hello-kubernetes-second.yaml** - Dummy application 2. // <- use this
- ~~**app-swimmpair.yaml** - **Production**~~ SwimmPair - example of LAMP web application w/ all necessary services. // <- don't use this
___ 
## Kubernetes Objects - general basic must-know
### Cluster Architecture
- Nodes - Master Node, Worker Nodes, (The Kubernetes API)
### Workload
- Deployments - define Pods and ReplicaSets
### Services, Load Balancing, and Networking
- Ingress
- Service (port, targetPort)
### Storage
- Volume - Persistent Volume
- Persistent Volume Claim
### Configuration
- Secret
### Tasks
- HorizontalPodAutoscaler - attached to Deployment




