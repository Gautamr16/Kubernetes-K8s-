# Kubernetes(K8s) Documentation
In this documentation, we cover the basics of Kubernetes **(K8s)** like key features, core concepts, Architecture, Installation and some basic  commands and practices.
![k](https://github.com/Gautamr16/Kubernetes-K8s-/assets/173404083/32d00da8-027b-443f-b71f-ca938103b2f5)
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
6. [Advanced Topics](#advanced-topics)
7. [Best Practices](#best-practices)
8. [Troubleshooting](#troubleshooting)
9. [Conclusion](#conclusion)
10. [References](#references)

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
- Efficiently schedules containers based on resource requirements and constraints.
- Ensures applications have the necessary resources to run optimally.

### Self-Healing Capabilities
- Automatically restarts failed containers.
- Replaces and reschedules containers when nodes fail.
- Kills containers that don't respond to health checks.
- Keeps non-ready containers from being advertised to clients.

### Horizontal Scaling
- Automatically or manually scales applications based on CPU utilization or other metrics.

### Service Discovery and Load Balancing
- Provides a single DNS name for a set of containers.
- Balances the load across multiple containers.

### Automated Rollouts and Rollbacks
- Manages deployment of new versions of applications.
- Can roll back to a previous version if issues are detected.

### Secret and Configuration Management
- Securely stores and manages sensitive information (passwords, tokens, SSH keys).
- Allows updating secrets and configurations without rebuilding container images.

### Storage Orchestration
- Automatically mounts the storage system of your choice.
- Supports local storage, public cloud providers, and network storage systems (NFS, iSCSI, Gluster, Ceph, Cinder, Flocker).

## Core Concepts

### Pods:
- The smallest and simplest Kubernetes object.
- Represents a single instance of a running process in your cluster.
- Encapsulates one or more containers, storage resources, a unique network IP, and options for how the containers should run.

### Services:
- An abstraction to define a logical set of Pods and a policy to access them.
- Allows decoupling of the service implementation from the client.

### Deployments:
- Provides declarative updates to applications.
- Describes an application’s life cycle, such as which images to use for the app, the number of Pod replicas, and the update strategy.

### ConfigMaps and Secrets:
- **ConfigMaps:** Provides a way to inject configuration data into Pods.
- **Secrets:** Designed to hold sensitive information, such as passwords and OAuth tokens.

### Namespaces:
- Provides a mechanism to isolate groups of resources within a single cluster.
- Useful in environments with multiple teams or projects.

## Kubernetes Architecture and Working
Kubernetes follows a client-server architecture. This setup includes a minimum of one master (control plane) node alongside several worker nodes, which serve as hosts for pods that contain the containers. The master node orchestrates the worker nodes, where the actual applications reside. </br>
![Kubernetes_architecture](https://github.com/Gautamr16/Kubernetes-K8s-/assets/173404083/a4ae7fd3-b23e-4d97-b48c-7962cf131159)
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

**Cloud Controller Manager:** 
- Manages cloud-specific controller logic.

### **Key Components of Worker Nodes:**
**Kubelet:** 
- Ensures containers are running in a Pod.

**Kube-Proxy:** 
- Maintains network rules on nodes.

**Container Runtime:** 
- Software responsible for running containers (e.g., Docker, containerd).

### Networking in Kubernetes:
Kubernetes networking addresses four primary concerns:

**Container-to-container** communication within a Pod </br>
**Pod-to-Pod** communication across nodes</br>
**Pod-to-Service** communication within the cluster</br>
**External-to-Service** communication with the outside world 

## The Data Plane
The data plane is responsible for running application workloads. It includes the following key components on each worker node:

### Components of the Data Plane
- **Pods:** 
    - The smallest and simplest Kubernetes object. A pod represents a single instance of a running process in your cluster and can contain one or more containers.

- **Kubelet:** 
    - Ensures the containers described in PodSpecs are running and healthy.

- **Container Runtime:** 
    - Runs the containers inside pods.

- **Kube-proxy:** 
    - Manages networking for pods, ensuring network traffic is correctly routed.

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
- Multiple Linux-based servers (VMs or physical machines)
- Ubuntu 20.04 or compatible Linux distribution
- 2 CPUs or more
- 2GB or more of RAM per node
- Full network connectivity between all nodes

## Step-by-Step Installation

### Step 1. Check if the system up-to-date using following command :
```
    sudo apt-get update
    sudo apt-get upgrade 
```
### Step 2. Kubernetes uses Docker as its container runtime. Install Docker on all nodes:
- After update system Now you can Install Docker using the following command:
``` 
    sudo apt install docker.io
```
- You’ll then get a prompt asking you to choose between y/n - choose y
```
    Do you want to continue? [Y/n] y
```
- Install all the dependency package using this command:
```
    sudo snap install docker
```
- check the version of Docker :
```
    gautamr@gautam:~$ docker -v
    Docker version 24.0.7, build 24.0.7-0ubuntu4
```
### step 3. Start and Enable Docker
``` 
sudo systemctl enable docker
```
 ```
 sudo service docker start
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
### Step 4. Install Kubernetes Components
- Installing Dependencies
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



## Advanced Topics

### Custom Resource Definitions (CRDs)
- Extend Kubernetes capabilities by defining custom resources.

### Helm Charts
- Package manager for Kubernetes applications.

### Persistent Storage
- Methods and best practices for managing persistent storage.

### Network Policies
- Define network traffic rules for Pods.

### Monitoring and Logging
- Tools and practices for monitoring and logging in Kubernetes.

### Security Best Practices
- Strategies to enhance security in Kubernetes environments.

## Best Practices

### Resource Management
- Efficiently managing resources in a Kubernetes cluster.

### Namespace Strategies
- Effective use of namespaces for organization and security.

### CI/CD Integration
- Integrating Kubernetes with CI/CD pipelines.

## Troubleshooting
### Common Issues and Solutions

#### 1. Pod Not Starting
- **Issue**: A pod is not starting or is stuck in `Pending` state.
- **Possible Causes**:
  - Insufficient resources.
  - Incorrect configuration.
- **Solution**:
  - Check pod status: ```kubectl get pods```
  - Describe pod for more details: ```kubectl describe pod <pod-name>```
  - Check resource availability: ```kubectl describe nodes```

#### 2. CrashLoopBackOff Error
- **Issue**: A pod is repeatedly crashing.
- **Possible Causes**:
  - Application error.
  - Resource limits too low.
- **Solution**:
  - View logs: ```kubectl logs <pod-name>```
  - Check container details: ```kubectl describe pod <pod-name>```

#### 3. Service Not Accessible
- **Issue**: A service is not accessible from within or outside the cluster.
- **Possible Causes**:
  - Incorrect service configuration.
  - Network issues.
- **Solution**:
  - Get service details: ```kubectl get services```
  - Describe service: ```kubectl describe service <service-name>```
  - Check endpoints: ```kubectl get endpoints```

#### 4. Node Not Ready
- **Issue**: A node is in `NotReady` state.
- **Possible Causes**:
  - Network issues.
  - Node resource exhaustion.
- **Solution**:
  - Get node status: ```kubectl get nodes```
  - Describe node: ```kubectl describe node <node-name>```

#### 5. Persistent Volume Not Bound
- **Issue**: A persistent volume (PV) is not binding to a persistent volume claim (PVC).
- **Possible Causes**:
  - Mismatched storage class.
  - Insufficient storage.
- **Solution**:
  - Check PV and PVC status: ```kubectl get pv``` and ```kubectl get pvc```
  - Describe PV and PVC for details: ```kubectl describe pv <pv-name>``` and ```kubectl describe pvc <pvc-name>```



### Debugging Techniques
- Techniques and tools for debugging issues in Kubernetes.

### Performance Tuning
- Tips for optimizing Kubernetes performance.

## Conclusion
- Kubernetes makes managing applications easier, improving deployment times, scalability, and reliability. It has a strong community and is widely used in modern cloud environments. Starting with basic concepts and gradually exploring more advanced features is a good way to learn and benefit from Kubernetes.

## References
<https://kubernetes.io/>
    
## Thank You!
