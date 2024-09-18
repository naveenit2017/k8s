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
Installations steps on Ubuntu:
-----------------------------
1.kubectl installation.
    1 sudo apt-get update
    2  kubectl version --client
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
#minikube start(By default Docker driver will use to create the K8S cluster)
#Minikube -->single node k8s cluster with one Master and slave node.
#kubectl get nodes (here we come to know the cluster information)

1.creating pod.yml from official site.
https://kubernetes.io/docs/concepts/workloads/pods/
#deploying the pod.yml
#kubectl create -f pod.yml
#kubectl get pods
#kubectl get pods -o wide

 kubectl create -f pod.yml
       kubectl get pods //get the pods information
       kubectl get svc  //will get the default svc
       kubectl get deploy //if any depolyments are there it will give.
       kubectl get pods -o wide //It will display the pod information.
       kubectl logs nginx
       kubectl describe pod nginx //

    #Now just take the pod ip and connect from the minikube.
       minikube ssh
       curl 10.244.0.3(pod ip)
       it will connect the nginx application from the minikiube machine.
#As of now pod is created then what next we need to do ?

--> Auto scaling and auto healing will get through deployment. 
Kubernetes Deployment:
----------------------
--Pod is a wrapper of container.
--To run the container will run the docker commands as arguments in commandline.
--Pod is a running specifiaction of container,pod wil have the one or more containers,because the actual container is depending on other one like LB,Aws  
      related info as well as network sharable and also storage also.
 --In pod everything will mentain in yml specifiaction files.
Auto Healing:
--------------
-Auto Healing we can acheive through deplyoments with zero down time,that it won't possible through container or pod.

-will mention deployment yml file kind argument insted of pod will use deployment.
The flow of deployment
 -->Deploy.yml--->Replication Set(Control Manager)number of replicas----->pod.
 --create deployment.yml file and deploy---
  kubectl get pods
  kubectl get deploy
  kubectl create -f deployment.yml
  kubectl get pods
  kubectl delete pod nginx-deployment-86dcfdf4c6-bl9p8
  kubectl get pods -o wide
  kubectl describe pod nginx
  kubectl describe pods
  kubectl get all
  minikube ssh //take nginx pod ip -->curl ip   //we are able to connect from only local machine.
  We can achieve replication by using deployment.
  kubectl get rs
  kubectl svc
  kubectl get svc
 -->RS(Cloud Controller) will provide the auto healing if the pod is down or termintaed before going to down it will create new pod.

# k8s
https://app.smartdraw.com/editor.aspx?templateId=5375d76a-18f5-49fc-bb02-78cfb46beb7e&flags=128#depoId=56018990&credID=-61926548.

Labels & Selectors:
-------------------
In Kubernetes, labels and selectors are mechanisms used to identify and organize resources. They play a key role in grouping, filtering, and managing objects like Pods, Services, and Deployments in a Kubernetes cluster.

Labels:
-------
Labels are key-value pairs that are attached to Kubernetes objects such as Pods, Services, Nodes, and more. Labels provide identifying metadata that can be used for selection, grouping, and organization of resources. They are arbitrary, meaning you can assign any key-value pair to suit your use case.
ex:
----
apiVersion: v1
kind: Pod
metadata:
  name: my-pod   //pod-name
  labels:
    app: web-server  //application name
    environment: production
spec:
  containers:
    - name: nginx
      image: nginx
  ex:selectors:
  apiVersion: v1
kind: Service
metadata:
  name: web-server-service
spec:
  selector:
    app: web-server
    environment: production
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80

# kubectl get pods --show-labels

Node Selector:-
---------------
In Kubernetes, a node selector is used to constrain Pods to specific nodes based on node labels. This is useful when you want to ensure that a Pod runs only on nodes that meet certain criteria, such as nodes with specific hardware or software characteristics.      

#kubectl get nodes --show-labels
 ReplicationController:
 ----------------------
A ReplicationController in Kubernetes ensures that a specified number of pod replicas are running at any given time.
It is responsible for maintaining the desired state of your Pods and ensuring that the specified number of replicas are always available.

While ReplicationControllers are still supported, they have largely been replaced by ReplicaSets in modern Kubernetes deployments, which offer more features and flexibility. However, understanding ReplicationControllers can still be useful for understanding the evolution of Kubernetes and for working with legacy configurations.
eg:
--
apiVersion: v1
kind: ReplicationController
metadata:
  name: nginx-rc
spec:
  replicas: 3
  selector:
    app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx-container
          image: nginx:latest
          ports:
            - containerPort: 80
A ReplicaSet in Kubernetes ensures that a specified number of pod replicas are running at any given time, similar to a ReplicationController. However, ReplicaSets provide more advanced features and flexibility compared to ReplicationControllers. ReplicaSets are commonly used with Deployments to manage rolling updates and rollbacks.

Deployment:
-----------
In Kubernetes, a Deployment is a resource object that manages and automates the process of deploying and updating applications. It abstracts much of the complexity around managing ReplicaSets and Pods by providing declarative updates, rolling updates, rollbacks, and scaling.
Key Features of a Deployment:
-----------------------------
1.Declarative Updates: Define the desired state of your application (number of replicas, container version, etc.), and Kubernetes will 
---------------------
ensure that the actual state matches it.
Rolling Updates: Allows you to update the application (e.g., new container version) without downtime by gradually replacing Pods.
---------------
Rollbacks: If something goes wrong during an update, you can easily rollback to a previous version.
----------
Self-healing: If Pods fail or go down, the Deployment controller ensures that new Pods are automatically created to maintain the desired 
-------------
replica count.
Scaling: Easily scale your application up or down by changing the number of replicas in the Deployment.
-------

When Kubernetes (K8s) deployments fail, it could be due to a variety of factors. Here are some common causes of failed Kubernetes deployments and strategies to handle them:
------------------------------------------
1. Image Pull Errors
---------------------
Cause: Kubernetes cannot pull the container image from the registry.

Resolution::
-----------
Check the image URL in the deployment file.
Ensure proper access to the image registry, and credentials are correctly configured (e.g., Docker registry, AWS ECR).
Verify that the image exists and is properly tagged.

2. Insufficient Resources (CPU/Memory):
---------------------------------------
Cause: Pods request more resources than what is available in the cluster.

Resolution:
-----------
Review resource limits and requests in the deployment YAML.
Scale the cluster or reduce resource requests/limits if too high.
Use resource quotas to prevent over-provisioning.

3. Failed Health Checks (Liveness/Readiness Probes):
---------------------------------------------------
Cause: Liveness or readiness probes are misconfigured or the app fails the checks.

Resolution:
Check if the endpoints used by the probes are functional.
Adjust the probe configurations (e.g., initial delay, timeout, failureThreshold).
Analyze application logs for deeper insights.

4. Failed ConfigMap/Secret Mounts:
----------------------------------
Cause: Pods fail to mount ConfigMaps or Secrets required by the application.

Resolution:
Ensure ConfigMaps and Secrets exist and are correctly referenced in the deployment file.
Verify the correct permissions and paths for mounts.

5. Networking Issues:
---------------------
Cause: Connectivity issues between services, DNS failures, or misconfigured network policies.
Resolution:
----------
Validate the service definitions (ClusterIP, NodePort, LoadBalancer).
Ensure DNS services are functional (check CoreDNS).
Review network policies to ensure correct ingress/egress traffic permissions.

6. Persistent Volume Claims (PVC) Failures:
-------------------------------------------
Cause: PVCs cannot be bound to a PersistentVolume (PV) or the PV is unavailable.

Resolution:-
------------
Verify storage class configurations and check if PVs exist for the requested storage.
Ensure the PVC is correctly configured and the storage is available.

7. CrashLoopBackOff:
--------------------
Cause: The application crashes and Kubernetes keeps restarting the pod.

Resolution:
-----------
Check pod logs (kubectl logs <pod-name>) for application errors.
Investigate readiness and liveness probes that might cause premature restarts.
Analyze the application’s environment (configurations, dependencies).

8.Service Account/Role Issues:
-------------------------------
Cause: The deployment lacks proper permissions due to misconfigured roles or service accounts.

Resolution:-
-----------
Verify the RoleBinding and ClusterRoleBinding configurations.
Ensure the service account used by the pod has the required permissions.

Kubernetes StatefulSets:
------------------------
Kubernetes StatefulSets are used to manage stateful applications, which require stable, unique network identities and persistent storage. Unlike Deployments, which are more suited for stateless applications, StatefulSets are designed to handle applications with persistent data.

Kubernetes Resources:
---------------------
Deployments: Typically used for stateless applications. Deployments manage replicas of pods and ensure that the desired number of pod 
-----------
instances are running. They handle rolling updates and rollbacks seamlessly.
Services: Provide load balancing and service discovery for stateless applications. They can distribute traffic across multiple instances.
--------
K8S Networking:
--------------
Kubernetes networking model is designed to provide a flexible, scalable, and consistent way for containers, pods, and services to communicate with each other. The networking model ensures that containers within a pod can communicate with each other and with other pods across the cluster seamlessly. It also provides mechanisms for exposing services to the outside world.

Key Concepts in Kubernetes Networking:
--------------------------------------
Pod Networking:
---------------
Each Pod Gets a Unique IP Address: Every pod in a Kubernetes cluster gets its own IP address, which allows containers within the pod to communicate with each other and with other pods using standard networking protocols.
Container-to-Container Communication: Containers within the same pod can communicate with each other via localhost (127.0.0.1), while 
------------------------------------
containers in different pods communicate using the pod’s IP address.
Cluster Networking:
-------------------
Flat Network: All pods can communicate with all other pods without Network Address Translation (NAT). This means there are no network boundaries within the cluster, allowing for direct communication between pods across different nodes.
Service Networking:
------------------
ClusterIP Services:- 
-------------------------
Services within the cluster are assigned a stable IP address and DNS name. These services use kube-proxy to route 
traffic to the appropriate pod endpoints based on the service selector.

NodePort Services: Allows exposing a service on a specific port on each node in the cluster. This makes the service accessible from outside the cluster by hitting any node’s IP address and the NodePort.
LoadBalancer Services: Provisioned through an external load balancer provided by cloud providers. This service is exposed via a public IP address that routes traffic to the nodes and services in the cluster.
Ingress: Provides HTTP and HTTPS routing to services based on URL paths and hostnames. It can be used to expose multiple services through a single external IP address.

Network Policies
----------------
Controlling Traffic: Kubernetes Network Policies allow you to define rules for controlling the traffic between pods and services. You can specify which pods can communicate with each other and which cannot.
DNS:
---
Service Discovery: 
------------------
Kubernetes provides built-in DNS for service discovery. Each service is assigned a DNS name, and DNS resolution is handled by CoreDNS (or kube-dns in some clusters). Pods can use these DNS names to discover and connect to services.
Overlay Networks:
----------------
Network Plugins: Kubernetes supports various networking plugins (CNI) that implement overlay networks, such as Calico, Flannel, and Weave. These plugins provide different capabilities for network policies, IP address management, and network segmentation.

Service:
-----------
In Kubernetes, a Service is an abstraction that defines a logical set of pods and a policy by which to access them. Services provide a stable network endpoint and load balancing for accessing a set of pods. Here’s an overview of different types of services and examples for each:
Types of Kubernetes Services:
-----------------------------
ClusterIP: Exposes the service on an internal IP within the cluster. This is the default service type and is used for communication between pods within the cluster.
NodePort: Exposes the service on a static port on each node’s IP address. This allows access to the service from outside the cluster.
LoadBalancer: Provisioned by cloud providers to create an external load balancer. This exposes the service to the outside world via a public IP address.

Headless Service: Provides service discovery without load balancing. It’s used to allow direct communication with individual pods.



