Docker : Docker is container platform and it is ephemenarol nature, i'e the containers life is very short 
Docker doesn't support few things:
1.The nature of problem with docker is a single host,If multiple usres are accessing the same container it will create a latency problem as well as require multipe containers 
but the docer single host there is resource problem will occure.
2.Docker Doesn't support the "Auto Scaling",If client require any festival season more containers to access theire application without latency there is no auto increasing and decresing.
3.It won't support the auto healing concept,If someone came and killed that container it won't come back again with manual intervention,if end user is using the same public ip then it is bottleneck to them
there is a down time also.
4.It doesn't support enterprise level support.

Kubernetes: K8s is container orchastartion platform.
-----------
1.K8s will support vesatile cloud support and it is a master follows slave architecture,Hence there muliple slave machines to increase the containers.
2.K8s will support the autoscaling through replication controller(replica sets--number of machines).
3.K8s will take care auto healing it any container is going down it will create automatically on other nodes.
4.K85 will support enterprise level support it has the more contributors to contribute the k8s.

Kubernetes Architecture:
-----------------------
K8S has two major  components.
1.Control Plane.
 i.API Server
 ii.Scheduler
 iii.etcd
 iv.Control Manager
 v.Cloud Control Manager(ingress controller)
 
2.Worker Plane
  i.Container Runtime(Docker)
  ii.Kubeproxy
  iii.Kublet

1.API Server:Expose to the external K8s and we can all it as heart of the k8s.
2.Scheduler: It will based on the API server iputs it will shedule the pods on nodes.
3.etcd: It will used for the storage purpose and backup everything will store in the form of key value pair.
4.Controller Manager : It will handler to the replicasets(slaves)
5.Cloud Controller Manager: If we require any cloud related services,k8s has provided the apis to implement there own cloud controller manager to versitele clouds.(eg:nginx--ingress controller)
1.Container Runtime:
ex:If compare with docker container,to run a java application container we require java runtime,Here container run time is Dockershim.
In kubernetes to run the pods we require a continer runtime,hence we are using container run time as Docker.
2.Kubelet: Kubelet is responsble to run the pods(like dockershim) based on the scheduler inputs, If pod is not working it will restart the pod and give response back to the API server,even if is not working create the new pods.
3.Kube-proxy:It will provide the network internally communicatins with pods and it will generates the ip address as well as hold the ip address list also.

To practice K8s:..
------------------
Prerequiste:
1.minikube,K3S,kind(k8s in Docker) and mickrok8s.
2.kubectl
3.docker
Installations teps on Ubuntu:
-----------------------------
1.kubectl installation.
    1 sudo apt-get update
    2  sudo apt-get install -y kubectl
    3  kubectl version --client
    4  sudo snap install kubectl
    5  sudo snap install kubectl --classic
    6  kubectl version --client
 2.Docker installation.
  sudo apt update
    sudo apt install apt-transport-https ca-certificates curl software-properties-common
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
    sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
    sudo apt update
    sudo apt install docker-ce
    sudo systemctl status docker
    sudo usermod -aG docker $USER
    docker --version
    
3.minikube
sudo apt update
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo chmod +x minikube-linux-amd64
sudo chmod +x minikube-linux-amd64
sudo usermod -aG docker $USER && newgrp docker
minikube start
minikube status
kubectl get pods
kubectl get nodes
-----------------------------------------------------------------------------------------------------------

Docker --> Container
K8S ---> Pod -->containers.
Note: In the docker to run we will pass all the arguments in command line only.
Pod is described a defination of how to run a container in k8s.
K8s everything will be YAML files.
The containers information will be available in pod.yml file.
In kuberenets everything will be mention yml files only.
..why pod having multiple containers.
      few advantages:
       i.Shared Network.
       ii.Shared Storage.
       
#Pod --> 1 or group of containers.
Pod --container-->Cluster IP, k8s allocate IPs through kube-proxy .
pod is a wrapper of container.

#Kubectl to communicate through commandline to K8s cluster
Deploying first Application :
-----------------------------
#minikube start(By default Docker driver will use to create the K8S cluster.
#Minikube -->single node k8s cluster with one Master and slave node.
#kubectl get nodes (here we come to know the cluster information)

1.creating pod.yml from official site.
https://kubernetes.io/docs/concepts/workloads/pods/
#deploying the pod.yml
#kubectl create -f pod.yml
#kubectl get pods
#kubectl get pods -o wide

# k8s
https://app.smartdraw.com/editor.aspx?templateId=5375d76a-18f5-49fc-bb02-78cfb46beb7e&flags=128#depoId=56018990&credID=-61926548
