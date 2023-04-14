## Key Concepts and Terminologies
**Kubernetes is an open-source container orchestration platform that automates the deployment, scaling, and management of containerized applications.**

- #### Cluster Architecture
  A Kubernetes cluster consists of a set of worker machines, called nodes, that run containerized applications. Every cluster has at least one worker node.
- #### Containers
- #### Workloads
- #### Services
- #### Load Balancing
- #### Networking
- #### Storage/Volumes
- #### Configuration
- #### Cluster Administration

## Setting up Kubernetes

### Creating a Cluster Using KIND 
[**K**ubernetes **IN** **D**ocker]

- ⚒️ | Install KIND using Homebrew 

  `brew install kind`
  
- ⚒️ | Install KubeCTL using homebrew 

  `brew install kubectl`
  
- ⚒️ | Configuration for Docker desktop.
    
    You need a minimum of 6GB of RAM dedicated to the virtual machine (VM) running the Docker engine. 8GB is recommended. 
    
    Docker > Preferences > Advanced > Memory 
    
- ⚒️ | Create First Cluster using KIND. you can use use the following basic flags: 

  `kind create cluster --name new-cluster --config env/manifests/`
    
    **--name** | to name the cluster
    
    **--image** | to declare a cluster image
    
    **--config** | to declare the config manifest[s] to be used. If declared, only the config manifests passed here will be used during the creation.
    
- ⚒️ | Setup some log outputting for the new cluster 

  `kind export logs /tmp/logs --name new-cluster`

  The structure of the logs dir generated will look similar to the following by default
  
   ```
    ├── docker-info.txt
    └── kind-control-plane/
        ├── containers
        ├── docker.log
        ├── inspect.json
        ├── journal.log
        ├── kubelet.log
        ├── kubernetes-version.txt
        └── pods/
    ```
- ⚒️ | Nice! Now delete the cluster, it's time to look at creating some manifests! [ YAML files ] 

  `kind delete cluster --name new-cluster`

## YAML Manifests

The minimal config requirements are:

```
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
```
