Kubernetes is the software behind the concept of container orchestration, there are many more but I am learning kubernetes right now.

Kubernetes is a container orchestrator to provision, manage, and scale apps. You can use it to manage the lifecycle of containerized apps in a cluster of nodes, which is a collection of worker machines such as VMs or physical machines.


***Kubernetes Cluster
***
### Control Plane

Every Kubernetes cluster requires a Control Plane node, the control plane's components make global decisions about the cluster (for example, scheduling), as well as detecting and responding to cluster events.
![[Pasted image 20221127115241.png]]

### Worker node

A worker machine that runs Kubernetes workloads. It can be a physical (bare metal) machine or a virtual machine (VM). Each node can host one or more pods. Kubernetes nodes are managed by a control plane
![[Pasted image 20221127115410.png]]

### Kubelet

An agent that runs on each node in the cluster. It makes sure that containers are running in a Pod.

The kubelet takes a set of PodSpecs that are provided through various mechanisms and ensures that the containers described in those PodSpecs are running and healthy. The kubelet doesn't manage containers which were not created by Kubernetes.
![[Pasted image 20221127115522.png]]

### **kube-proxy

kube-proxy is a network proxy that runs on each node in your cluster, implementing part of the Kubernetes Service concept.

kube-proxy maintains network rules on nodes. These network rules allow network communication to your Pods from network sessions inside or outside of your cluster.

kube-proxy uses the operating system packet filtering layer if there is one and it's available. Otherwise, kube-proxy forwards the traffic itself.

![[Pasted image 20221127115831.png]]

### container runtime

The container runtime is the software that is responsible for running containers.

Kubernetes supports several container runtimes: containerd, CRI-O, and any implementation of the Kubernetes CRI (Container Runtime Interface).
![[Pasted image 20221127115908.png]]

### Cluster

A cluster is a group of nodes, where a node can be a physical machine or a virtual machine. Each of the nodes will have the container runtime (Docker) and will also be running a kubelet service, which is an agent that takes in the commands from the Master controller (more on that later) and a Proxy, that is used to proxy connections to the Pods from another component (Services, that we will see later).

Our control plane which can be made highly available will contain some unique roles compared to the worker nodes, the most important will be the kube API server, this is where any communication will take place to get information or push information to our Kubernetes cluster.

### Kube API-Server

The Kubernetes API server validates and configures data for the API objects which include pods, services, replication controllers, and others. The API Server services REST operations and provide the frontend to the cluster's shared state through which all other components interact.

#### Scheduler

The Kubernetes scheduler is a control plane process which assigns Pods to Nodes. The scheduler determines which Nodes are valid placements for each Pod in the scheduling queue according to constraints and available resources. The scheduler then ranks each valid Node and binds the Pod to a suitable Node.

### Controller Manager

The Kubernetes controller manager is a daemon that embeds the core control loops shipped with Kubernetes. In applications of robotics and automation, a control loop is a non-terminating loop that regulates the state of the system. In Kubernetes, a controller is a control loop that watches the shared state of the cluster through the apiserver and makes changes attempting to move the current state towards the desired state.

### etcd 

Consistent and highly-available key value store used as Kubernetes' backing store for all cluster data.

###
![[Pasted image 20221127121705.png]]
#### kubectl

To manage this from a CLI point of view we have kubectl, kubectl interacts with the API server.

The Kubernetes command-line tool, kubectl, allows you to run commands against Kubernetes clusters. You can use kubectl to deploy applications, inspect and manage cluster resources, and view logs.
![[Pasted image 20221127124341.png]]
### Pods

A Pod is a group of containers that form a logical application. E.g. If you have a web application that is running a NodeJS container and also a MySQL container, then both these containers will be located in a single Pod. A Pod can also share common data volumes and they also share the same networking namespace. Remember that Pods are ephemeral and they could be brought up and down by the Master Controller. Kubernetes uses a simple but effective means to identify the Pods via the concepts of Labels (name – values).

-   Pods handle Volumes, Secrets, and configuration for containers.
    
-   Pods are ephemeral. They are intended to be restarted automatically when they die.
    
-   Pods are replicated when the app is scaled horizontally by the ReplicationSet. Each Pod will run the same container code.
    
-   Pods live on Worker Nodes.
![[Pasted image 20221127124537.png]]
### Deployments

-   You can just decide to run Pods but when they die they die.
    
-   A Deployment will enable your pod to run continuously.
    
-   Deployments allow you to update a running app without downtime.
    
-   Deployments also specify a strategy to restart Pods when they die

![[Pasted image 20221127124821.png]]
-
### ReplicaSets

-   The Deployment can also create the ReplicaSet
    
-   A ReplicaSet ensures your app has the desired number of Pods
    
-   ReplicaSets will create and scale Pods based on the Deployment
    
-   Deployments, ReplicaSets, and Pods are not exclusive but can be
    

### StatefulSets

-   Does your App require you to keep information about its state?
    
-   A database needs state
    
-   A StatefulSet’s Pods are not interchangeable.
    
-   Each pod has a unique, persistent identifier that the controller maintains over any rescheduling.
![[Pasted image 20221127125005.png]]

### DaemonSets

-   DaemonSets are for continuous process
    
-   They run one Pod per Node.
    
-   Each new node added to the cluster gets a pod started
    
-   Useful for background tasks such as monitoring and log collection
    
-   Each pod has a unique, persistent identifier that the controller maintains over any rescheduling.
![[Pasted image 20221127125103.png]]

### Services

-   A single endpoint to access Pods
    
-   a unified way to route traffic to a cluster and eventually to a list of Pods.
    
-   By using a Service, Pods can be brought up and down without affecting anything.
![[Pasted image 20221127125227.png]]
