# Wanderlust Deployment on Kubernetes

### In this project, we will learn about how to deploy wanderlust application on Kubernetes.

### Pre-requisites to implement this project:
-  Create 2 AWS EC2 instance (Ubuntu) with instance type t2.medium and root volume 29GB.
-  Setup <a href="https://github.com/DevMadhup/wanderlust/blob/devops/kubernetes/kubeadm.md"><u> Kubeadm </a></u>

#
## Steps for Kubernetes deployment:

1) Become root user :
```bash
sudo su
```

#
2) Clone code from remote repository (GitHub) :
```bash
git clone https://github.com/shilemon/wanderlust.git
```

#
3) Verify nodes are in ready state or not :
```bash
kubectl get nodes
```
<img width="576" height="60" alt="nodes" src="https://github.com/user-attachments/assets/692a45ae-aff6-4062-ae27-eaff12eeedfb" />


#
4) Create kubernetes namespace :
```bash
kubectl create namespace wanderlust
```
<img width="722" height="59" alt="namespace create" src="https://github.com/user-attachments/assets/609cbdc4-4425-4e86-9f6c-ed25d2c9c475" />

#
5) Update kubernetes config context : 
```bash
kubectl config set-context --current --namespace wanderlust
```
<img width="926" height="30" alt="context wanderlust" src="https://github.com/user-attachments/assets/335a776d-135b-4d79-ae38-44c64ebac6ab" />

#
6) Enable DNS resolution on kubernetes cluster :

- Check coredns pod in kube-system namespace and you will find <i> Both coredns pods are running on master node </i>

```bash
kubectl get pods -n kube-system -o wide | grep -i core
```
<img width="843" height="76" alt="get-coredns" src="https://github.com/user-attachments/assets/6bcdf8a1-5baa-4176-a6e5-d9e139cf96a9" />

- Above step will run coredns pod on worker node as well for DNS resolution

```bash
kubectl edit deploy coredns -n kube-system -o yaml
```
<i> Make replica count from 2 to 4 </i>

<img width="847" height="527" alt="edit-coredns" src="https://github.com/user-attachments/assets/f0022554-be6b-4fd6-ba38-97849b012b72" />

#
7) Navigate to frontend directory :
```bash
cd frontend
```

#
8) Edit .env.docker file and change the public IP Address with your worker node public IP :
```bash
vi .env.docker
```
<img width="377" height="22" alt="frontend env docker" src="https://github.com/user-attachments/assets/e82f19fa-4d2f-4a30-8c49-9797e9e89028" />

#
9) Build frontend docker image : 
```bash
docker build -t madhupdevops/frontend-wanderlust:v2.1.8 .
```
<img width="890" height="416" alt="docker frontend build" src="https://github.com/user-attachments/assets/8f542a1a-484f-4d6c-b134-b9a6af5877f4" />

#
10) Navigate to backend directory :
```bash
cd ../backend/
```

#
11) Open .env.docker file and edit below variables : 

    - MONGODB_URI: \<your-mongodb-servicename>
    - REDIS_URL: \<your-redis-servicename>
    - FRONTEND_URL: \<your-workernode-publicIP>

> Note: To get service names, check <u>mongodb.yaml, redis.yaml</u>

<img width="779" height="221" alt="backend env docker" src="https://github.com/user-attachments/assets/8d008c94-713b-486c-88c5-36efb3db7511" />

#
12) Build backend docker image : 
```bash
docker build -t madhupdevops/backend-wanderlust:v2.1.8 .
```
![Backend dockerfile](https://github.com/DevMadhup/wanderlust/blob/devops/kubernetes/assets/docker%20backend%20build.png)

#
13) Check docker images:
```bash
docker images
```
![docker images](https://github.com/DevMadhup/wanderlust/blob/devops/kubernetes/assets/docker%20images.png)

#
14) Login to DockerHub and push image to DockerHub
```bash
docker login
```

```bash
docker push emon110852/frontend-wanderlust:latest
docker push emon110852/backend-wanderlust:latest
```

#
15) Once, Image is pushed to DockerHub, navigate to kubernetes directory
```bash
cd ../kubernetes
```

#
16) Apply manifests file the below order:

    - Create persistent volume :
    ```bash
    kubectl apply -f persistentVolume.yaml 
    ```

    - Create persistent volume Claim :
    ```bash
    kubectl apply -f persistentVolumeClaim.yaml 
    ```

    - Create MongoDB deployment and service :
    ```bash
    kubectl apply -f mongodb.yaml 
    ```

    - Create Redis deployment and service :
    > Note: Wait for 3-4 mins to get mongodb, redis pods and service should be up, otherwise backend-service will not connect.
    ```bash
    kubectl apply -f redis.yaml 
    ```

    - Create Backend deployment and service :
    ```bash
    kubectl apply -f backend.yaml 
    ```
<img width="1046" height="67" alt="image" src="https://github.com/user-attachments/assets/f4c64765-ba6b-471c-a278-02cf8284ab53" />

    - Create Frontend deployment and service :
    ```bash
    kubectl apply -f frontend.yaml
    ```
<img width="1053" height="64" alt="image" src="https://github.com/user-attachments/assets/f971b030-5626-4bf8-b9e4-119a8a57e367" />

#
17)  Check all deployments and services :
```bash 
kubectl get all
```
<img width="1556" height="547" alt="image" src="https://github.com/user-attachments/assets/04472c45-620f-467d-bcbf-b0f452f96757" />

18) Check logs for all the pods :
> Note: This is mandatory to ensure all pods and services are connected or not, if not then recreate deployments
```bash
kubectl logs <pod-name>
```

20) Navigate to chrome and access your application at 31000 port :
```bash
http://<your-workernode-publicip>:31000/
<img width="1911" height="807" alt="image" src="https://github.com/user-attachments/assets/b2fb7d25-5f2f-4f24-82d5-b577771d9dd9" />
