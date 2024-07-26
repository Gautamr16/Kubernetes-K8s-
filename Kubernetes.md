# Kubernetes(K8s) Documentation
In this documentation we covers basics of Kubernetes **(K8s)** like key features, core concepts, Architecture, Installation and some basic  commands.
![k](https://github.com/Gautamr16/Kubernetes-K8s-/assets/173404083/5bdb2634-84cc-4fd0-b80d-70d12d3b7aa0)

## Table of Contents

1. [Introduction](#introduction)
2. [Key Features](#key-features)
3. [Core Concepts](#core-concepts)
4. [Kubernetes Architecture and Working](#kubernetes-architecture-and-working)
    - [The Control Plane](#the-control-plane)
   - [The Data Plane ](#the-data-plane)
5. [Getting Started](#getting-started)
   - [Installation](#installation)
   - [Basic Commands](#basic-commands)
6. [Conclusion](#conclusion)
7. [References](#references)

## Introduction

Kubernetes (K8s) is an open-source tool that helps manage containerized applications. It automates the deployment, scaling, and operation of apps, making sure they run smoothly.

### Why is Kubernetes Called K8s?

Kubernetes is called "K8s" because there are eight letters between the "K" and the "s" in the word "Kubernetes." It’s a way to shorten the word and make it easier to write.

### *History and Development*
#### Origins:
- Kubernetes was created by Google. It uses Google’s experience in running large-scale systems.

#### Open Source:
- In June 2014, Google made Kubernetes open source, allowing anyone to use and improve it.

#### Community Growth:
- It quickly became popular and is now supported by a large community and many companies.

## Key Features

### Automated Scheduling
- Automated scheduling in Kubernetes is the process by which the Kubernetes scheduler automatically assigns Pods (units of work) to Nodes (machines) in a cluster. This ensures efficient resource use and balanced workloads.

### Self-Healing Capabilities
- Automatically restarts failed containers.
- Replaces and reschedules containers when nodes fail.
- Kills containers that don't respond to health checks.

### Horizontal Scaling
- Automatically or manually scales applications based on CPU utilization.

### Load Balancing
- Balances the load across multiple containers.

### Automated Rollouts and Rollbacks
- Manages deployment of new versions of applications.
- Can roll back to a previous version if issues are detected.

## Core Concepts

### Pods:
- The smallest and simplest Kubernetes object.
- Represents a single instance of a running process in your cluster.
- Encapsulates one or more containers, storage resources, a unique network IP, and options for how the containers should run.

### Services:
- Services are a way to manage how Pods (containers) are accessed.
- Allows decoupling of the service implementation from the client.

## Kubernetes Architecture and Working
Kubernetes follows a client-server architecture. This setup includes a minimum of one master (control plane) node alongside several worker nodes, which serve as hosts for pods that contain the containers. The master node orchestrates the worker nodes, where the actual applications reside. </br>
![Kubernetes_architecture](https://github.com/Gautamr16/Kubernetes-K8s-/assets/173404083/fa33e633-eee8-4588-ad16-a9852d5ea98f)

## The Control Plane
### Key Components of the Master Node:
**API Server:** 
- Acts as the frontend of the Kubernetes control plane. It exposes the Kubernetes API.

**etcd:** 
- Consistent and highly-available key-value store used as Kubernetes' backing store for all cluster data.

**Scheduler:**
- Watches for newly created Pods and assigns them to nodes.

**Controller Manager:** 
- Runs controller processes.

### Networking in Kubernetes:
Kubernetes networking addresses four primary concerns:

**Container-to-container** communication within a Pod </br>
**Pod-to-Pod** communication across nodes</br>
**Pod-to-Service** communication within the cluster</br>
**External-to-Service** communication with the outside world 

## The Data Plane
The data plane is responsible for running application workloads. It includes the following key components on each worker node:

### Key Components of Worker Nodes
- **Pods:** 
    - The smallest and simplest Kubernetes object. A pod represents a single instance of a running process in your cluster and can contain one or more containers.

- **Kubelet:**
    - Ensures containers are running in a Pod.

- **Container Runtime:** 
    - Runs the containers inside pods.
    - Software responsible for running containers (e.g., Docker, container d).

- **Kube-proxy:** 
    - Maintains network rules on nodes.

## Getting Started

### Installation
### Goals
Install a Docker container and then install Kubernetes with two nodes: 
- How to install Docker
- How to install Kubernetes
- How to configure a master and two worker node
- How to join a worker node to a Kubernetes cluster

### Prerequisites
Before you begin, ensure you have the following:
- Hostname: gautam
- Operating System: Ubuntu 24.04 LTS
- Kernel: Linux 6.8.0-38-generic
- Architecture: x86-64
  
Hardware Requirements:
- CPUs: At least 2
- RAM: At least 2GB

## Step-by-Step Installation

### Step 1. Check if the system up-to-date using following command :
```
    sudo apt-get update
    sudo apt-get upgrade 
```
### Step 2. Kubernetes uses Docker as its container runtime. Install Docker on all nodes:
- Set up Docker's `apt` repository.
``` 
    sudo apt-get install ca-certificates curl
    sudo install -m 0755 -d /etc/apt/keyrings
    sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
    sudo chmod a+r /etc/apt/keyrings/docker.asc
```
-  Add the repository to Apt sources:
```
    echo \
    "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
    $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
    sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
- To install the latest version of Docker Engine:
```
    sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
- check the version of Docker :
```
    gautamr@gautam:~$ docker -v
    Docker version 24.0.7, build 24.0.7-0ubuntu4
```
### step 3. Start and Enable Docker
``` 
sudo systemctl start docker
```
 ```
 sudo systemctl enable docker
 ```
- Check the status of docker service :
```
gautamr@gautam:~$ sudo service docker status
● docker.service - Docker Application Container Engine
     Loaded: loaded (/usr/lib/systemd/system/docker.service; enabled; preset: enabled)
     Active: active (running) since Fri 2024-06-21 23:28:26 IST; 1min 3s ago
TriggeredBy: ● docker.socket
       Docs: https://docs.docker.com
   Main PID: 102078 (dockerd)
      Tasks: 10
     Memory: 29.9M (peak: 30.5M)
        CPU: 289ms
     CGroup: /system.slice/docker.service
             └─102078 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock

Jun 21 23:28:25 gautam systemd[1]: Starting docker.service - Docker Application Container Engine...
Jun 21 23:28:25 gautam dockerd[102078]: time="2024-06-21T23:28:25.760393471+05:30" level=info msg="Starting up"
Jun 21 23:28:25 gautam dockerd[102078]: time="2024-06-21T23:28:25.763334487+05:30" level=info msg="detected 127.0.0.53 nameserver, assuming systemd-resolved,>
Jun 21 23:28:26 gautam dockerd[102078]: time="2024-06-21T23:28:26.043214895+05:30" level=info msg="Loading containers: start."
Jun 21 23:28:26 gautam dockerd[102078]: time="2024-06-21T23:28:26.261444552+05:30" level=info msg="Default bridge (docker0) is assigned with an IP address 17>
Jun 21 23:28:26 gautam dockerd[102078]: time="2024-06-21T23:28:26.321416672+05:30" level=info msg="Loading containers: done."
Jun 21 23:28:26 gautam dockerd[102078]: time="2024-06-21T23:28:26.337990034+05:30" level=info msg="Docker daemon" commit=de5c9cf containerd-snapshotter=false>
Jun 21 23:28:26 gautam dockerd[102078]: time="2024-06-21T23:28:26.338149917+05:30" level=info msg="Daemon has completed initialization"
Jun 21 23:28:26 gautam dockerd[102078]: time="2024-06-21T23:28:26.374636912+05:30" level=info msg="API listen on /run/docker.sock"
Jun 21 23:28:26 gautam systemd[1]: Started docker.service - Docker Application Container Engine.
```
### Step 4. Install Kubernetes Components and configuration
- **All in one single Node installation**
- with all-in-one, all the master and worker node/components are installed on a single node. This is very useful for learning and testing. This type should not be used in production. Minikube is one such example.
- **Installing Dependencies**
- - kubectl: Command-line tool for Kubernetes.
- - Minikube: Tool for running a local Kubernetes cluster.

**Install kubectl**
- Download kubectl
```
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
```
- Output:
```
gautamr@gautam:~$ curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   138  100   138    0     0    415      0 --:--:-- --:--:-- --:--:--   416
100 49.0M  100 49.0M    0     0  3285k      0  0:00:15  0:00:15 --:--:-- 3744k

```
- Install kubectl:
```
chmod +x kubectl
sudo mv kubectl /usr/local/bin/
```
- Verify installation:
```
kubectl version --client
```
- Output:
```
gautamr@gautam:~$ kubectl version --client
Client Version: v1.30.2
Kustomize Version: v5.0.4-0.20230601165947-6ce0bf390ce3
```
**Install Minikube**
- Download Minikube:
```
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
```
- Output:
```
gautamr@gautam:~$ curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 91.1M  100 91.1M    0     0  4313k      0  0:00:21  0:00:21 --:--:-- 3837k
```
- Install Minikube:
```
chmod +x minikube-linux-amd64
sudo mv minikube-linux-amd64 /usr/local/bin/minikube
```
- Verify installation:
```
minikube version
```
- Output:
```
gautamr@gautam:~$ minikube version
minikube version: v1.33.1
commit: 5883c09216182566a63dff4c326a6fc9ed2982ff
```
### Step 5. Start Minikube
- start Minikube:
```
minikube start
```
- Output:
```
gautamr@gautam:~$ minikube start
😄  minikube v1.33.1 on Ubuntu 24.04
✨  Using the docker driver based on existing profile
👍  Starting "minikube" primary control-plane node in "minikube" cluster
🚜  Pulling base image v0.0.44 ...
🏃  Updating the running docker "minikube" container ...
🐳  Preparing Kubernetes v1.30.0 on Docker 26.1.1 ...
🔎  Verifying Kubernetes components...
    ▪ Using image gcr.io/k8s-minikube/storage-provisioner:v5
🌟  Enabled addons: storage-provisioner, default-storageclass
🏄  Done! kubectl is now configured to use "minikube" cluster and "default" namespace by default
```
- Check cluster status:
```
minikube status
```
- Output:
```
gautamr@gautam:~$ minikube status
minikube
type: Control Plane
host: Running
kubelet: Running
apiserver: Running
kubeconfig: Configured
```
### Step 6: Use kubectl to Interact with Minikube
- Get cluster information:
```
kubectl cluster-info
```
- Output:
```
gautamr@gautam:~$ kubectl cluster-info
Kubernetes control plane is running at https://192.168.49.2:8443
CoreDNS is running at https://192.168.49.2:8443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
```
- Get nodes:
```
kubectl get nodes
```
- Output:
```
gautamr@gautam:~$ kubectl get nodes
NAME       STATUS   ROLES           AGE     VERSION
minikube   Ready    control-plane   7m38s   v1.30.0
```
### Step 7: Manifest file:
#### Steps for write a Kubernetes Manifest Using Vim
- **Step 1:** Open the Terminal
- **Step 2:** Create or Open a Manifest File
- - ` vim pod1.yaml ` or `vim pod1.yml`
- **Step 3:** Enter Insert Mode
- -  By default, `vim `opens in command mode. To start typing or editing the file, you need to switch to insert mode. Press `i` to enter insert mode.
- **Step 4:** Write Your Manifest
```
kind: Pod
apiVersion: v1
metadata:
  name: testpod1
spec:
  containers:
    - name: c00
      image: ubuntu
      command: ["/bin/bash", "-c", "while true; do echo Hello-gautam; sleep 5; done"]
```
- **Step 5:** Save and Exit
- - Once you have finished writing the manifest, exit insert mode by pressing `Esc`.
- - To save the file and exit vim, type: `:wq`

**Apply a Manifest**
- `kubectl apply -f <manifest-file>`
``` 
gautamr@gautam:~$ kubectl apply -f pod2.yml 
pod/testpod1 created
```
**Get Resources**
- `kubectl get pods`

```
gautamr@gautam:~$ kubectl get pods
NAME       READY   STATUS    RESTARTS   AGE
testpod1   1/1     Running   0          5m44
```
**Get Resources with Wide Output**
- `kubectl get pods -o wide`

```
gautamr@gautam:~$ kubectl get pods -o wide
NAME       READY   STATUS    RESTARTS   AGE     IP            NODE       NOMINATED NODE   READINESS GATES
testpod1   1/1     Running   0          7m26s   10.244.0.28   minikube   <none>           <none>
```
**View Logs**
- `kubectl logs -f  <pod-name>`

**Describe a Pod:**
- `kubectl describe pod <pod-name>`
```
gautamr@gautam:~$ kubectl describe pod testpod1
Name:             testpod1
Namespace:        default
Priority:         0
Service Account:  default
Node:             minikube/192.168.49.2
Start Time:       Thu, 25 Jul 2024 13:48:40 +0530
Labels:           <none>
Annotations:      <none>
Status:           Running
IP:               10.244.0.28
IPs:
  IP:  10.244.0.28
Containers:
  c00:
    Container ID:  docker://a1aed8ac2e119ca10f99861addf61c9f814e304b2b781aefc10c07052b98ba69
    Image:         ubuntu
    Image ID:      docker-pullable://ubuntu@sha256:2e863c44b718727c860746568e1d54afd13b2fa71b160f5cd9058fc436217b30
    Port:          <none>
    Host Port:     <none>
    Command:
      /bin/bash
      -c
      while true; do echo Hello-gautam; sleep 5; done
    State:          Running
      Started:      Thu, 25 Jul 2024 13:48:43 +0530
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-bxqxr (ro)
Conditions:
  Type                        Status
  PodReadyToStartContainers   True 
  Initialized                 True 
  Ready                       True 
  ContainersReady             True 
  PodScheduled                True 
Volumes:
  kube-api-access-bxqxr:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    ConfigMapOptional:       <nil>
    DownwardAPI:             true
QoS Class:                   BestEffort
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type    Reason     Age   From               Message
  ----    ------     ----  ----               -------
  Normal  Scheduled  12m   default-scheduler  Successfully assigned default/testpod1 to minikube
  Normal  Pulling    12m   kubelet            Pulling image "ubuntu"
  Normal  Pulled     12m   kubelet            Successfully pulled image "ubuntu" in 2.919s (2.919s including waiting). Image size: 78050118 bytes.
  Normal  Created    12m   kubelet            Created container c00
  Normal  Started    12m   kubelet            Started container c00

```
### Step 8: Cleanup
- Delete the pod:
```
kubectl delete pod (podname)
```
```
gautamr@gautam:~$ kubectl delete pod testpod1
pod "testpod1" deleted
```
- Stop Minikube:
```
minikube stop
```
- Output:
```
gautamr@gautam:~$ minikube stop
✋  Stopping node "minikube"  ...
🛑  Powering off "minikube" via SSH ...
🛑  1 node stopped.
```
- Delete Minikube cluster:
```
minikube delete
```
### Basic Commands

- List all pods
```kubectl get pods```

- List all services
```kubectl get services```

- List all deployments 
```kubectl get deployments```

- Create resources from a YAML file
```kubectl create -f <file.yaml>```

- Apply changes defined in a YAML file
```kubectl apply -f <file.yaml>```

- Delete resources defined in a YAML file
```kubectl delete -f <file.yaml>```

- Delete a pod
```kubectl delete pod <pod-name>```

- Delete a service
```kubectl delete service <service-name>```

- Delete a deployment
```kubectl delete deployment <deployment-name>```

## Conclusion
- Kubernetes makes managing applications easier, improving deployment times, scalability, and reliability. It has a strong community and is widely used in modern cloud environments. Starting with basic concepts and gradually exploring more advanced features is a good way to learn and benefit from Kubernetes.

## References
<https://docs.docker.com/get-docker/>
<br>
<https://kubernetes.io/>

## Thank You!
