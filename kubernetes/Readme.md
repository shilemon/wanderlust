Wanderlust Deployment on Kubernetes
In this project, we will learn about how to deploy wanderlust application on Kubernetes.
Pre-requisites to implement this project:
Create 2 AWS EC2 instance (Ubuntu) with instance type t2.medium and root volume 29GB.
Setup Kubeadm
Steps for Kubernetes deployment:
Become root user :
sudo su
Clone code from remote repository (GitHub) :
git clone -b devops https://github.com/DevMadhup/wanderlust.git
Verify nodes are in ready state or not :
kubectl get nodes
Alt text

Create kubernetes namespace :
kubectl create namespace wanderlust
Namespace

Update kubernetes config context :
kubectl config set-context --current --namespace wanderlust
Update context

Enable DNS resolution on kubernetes cluster :
Check coredns pod in kube-system namespace and you will find Both coredns pods are running on master node
kubectl get pods -n kube-system -o wide | grep -i core
Alt text

Above step will run coredns pod on worker node as well for DNS resolution
kubectl edit deploy coredns -n kube-system -o yaml
Make replica count from 2 to 4

replica 4

Navigate to frontend directory :
cd frontend
Edit .env.docker file and change the public IP Address with your worker node public IP :
vi .env.docker
IP

Build frontend docker image :
docker build -t madhupdevops/frontend-wanderlust:v2.1.8 .
Dockerfile frontend

Navigate to backend directory :
cd ../backend/
Open .env.docker file and edit below variables :

MONGODB_URI: <your-mongodb-servicename>
REDIS_URL: <your-redis-servicename>
FRONTEND_URL: <your-workernode-publicIP>
Note: To get service names, check mongodb.yaml, redis.yaml

Backend env file

Build backend docker image :
docker build -t madhupdevops/backend-wanderlust:v2.1.8 .
Backend dockerfile

Check docker images:
docker images
docker images

Login to DockerHub and push image to DockerHub
docker login
docker login

docker push madhupdevops/frontend-wanderlust:v2.1.8
docker push madhupdevops/backend-wanderlust:v2.1.8
Once, Image is pushed to DockerHub, navigate to kubernetes directory
cd ../kubernetes
Apply manifests file the below order:

Create persistent volume :
kubectl apply -f persistentVolume.yaml 
Peristent volume

Create persistent volume Claim :
kubectl apply -f persistentVolumeClaim.yaml 
Peristent volume Claim

Create MongoDB deployment and service :
kubectl apply -f mongodb.yaml 
MongoDb

Create Redis deployment and service :
Note: Wait for 3-4 mins to get mongodb, redis pods and service should be up, otherwise backend-service will not connect.

kubectl apply -f redis.yaml 
Redis

Create Backend deployment and service :
kubectl apply -f backend.yaml 
Backend

Create Frontend deployment and service :
kubectl apply -f frontend.yaml
Frontend

Check all deployments and services :
kubectl get all
all deployments and services

Check logs for all the pods :
Note: This is mandatory to ensure all pods and services are connected or not, if not then recreate deployments

kubectl logs <pod-name>
Navigate to chrome and access your application at 31000 port :
http://<your-workernode-publicip>:31000/
<img width="1919" height="845" alt="Screenshot 2025-11-11 221158" src="https://github.com/user-attachments/assets/9ffdb6b8-f344-4c8c-a47a-aeb9b7913932" />
