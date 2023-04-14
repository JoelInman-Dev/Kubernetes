# Setting up a Kubernetes Cluster

## Creating a Cluster using KIND
[**K**ubernetes **IN** **D**ocker]

⚒️ - Install KIND using Homebrew 

  `brew install kind`
  
⚒️ - Install KubeCTL using homebrew 

  `brew install kubectl`
  
⚒️ - Configuration for Docker desktop.
    
    You need a minimum of 6GB of RAM dedicated to the virtual machine (VM) running the Docker engine. 8GB is recommended. 
    
    Docker > Preferences > Advanced > Memory 
    
⚒️ - Let's jump straight in and Create my First Cluster using KIND. You can use use the following basic flags: 

  `kind create cluster --name new-cluster --config=/env/manifests/`
    
    **--name** | to name the cluster
    
    **--image** | to declare a cluster image
    
    **--config** | to declare the config manifest[s] to be used. If declared, only the config manifests passed here will be used during the creation.
    
⚒️ - I'll practice setting up some log outputting for the new cluster into a local dir within the project and commit it.

  `kind export logs ./.tmp/logs --name new-cluster`

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
⚒️ - Nice! Now delete the cluster, it's time to look at creating some manifests! [ YAML files ] 

  `kind delete cluster --name new-cluster`

## YAML Manifests

The minimal cluster config requirements are literally:

```
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
```

To deploy an application in Kubernetes, you first need to create **Deployment** and **Services** manifests in YAML files. 
Next, you apply the manifests to your Kubernetes cluster using the `kubectl apply -f /path/to/manifests/` command, now, verify that your application’s pods are running with `kubectl get pods`, and test the Services with `kubectl get services` [svc for short], and attempt accessing the service using a web browser or a tool like cURL. 
luckily, there's also a bunch of tools available that can help to simplify any application deployment in Kubernetes, such as 'Helm Charts' and 'Kubernetes Operators'.

#### *It's important to understand at this point, that any parameters passed in your CLI invocation will take priority over the same parameter, if declared within your config manifest.*

To get started, I am going to want to create the following Manifests: [My completed manifests in Kubernetes/manifests](https://github.com/joelinman-nxp/Kubernetes/manifests)

- **config.yaml** // This will declare the configuration for the KIND cluster.
- **deploy.yaml** // This will instruct Kubernetes on how to create and update instances of my application.

### A quick note on deployments.
Once the application instances are created, a Kubernetes Deployment controller will continuously monitor those instances. Should a Node hosting an instance die/crash or get deleted, the Deployment controller automatically replaces the instance with an instance on another Node in the cluster. This effectively provides a sort of self-healing mechanism to address any unexpected machine failure or maintenance.

Time to go study further and then create my first Deployment manifest!

