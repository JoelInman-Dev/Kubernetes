# Key Concepts and Terminologies
**Kubernetes is an open-source container orchestration platform that automates the deployment, scaling, and management of containerized applications.**

### Cluster Architecture
A Kubernetes cluster consists of a set of worker machines, called nodes, that run containerized applications. Kubernetes runs the workload by placing containers into pods to run on the nodes within a given cluster.
*Every cluster has at least one node.* In clusters that do have only 1 node, the control-plane is also the worker! Nodes within a cluster can be provided with lot's of customisable parameters, for example: you can provide a node with labels which can be used as a node selector parameter when it comes to attaching pods to specific nodes.

### Containers
A container is a lightweight way of running isolated software applications on a host operating system without the need for a full-blown virtual machine. Containers  run isolated processes, with their own file system, network, and resources. Containers can start up very quickly and are generally more efficient in their use of resources making them more suited to running single applications or microservices that can be quickly deployed and scaled up or down as needed.

### Workloads
Workloads in Kubernetes refer to the applications or services that are deployed and managed by the platform.
There are different types of workloads that can be deployed in Kubernetes, including:

- Deployment: This is probably the most common type of workload in Kubernetes. A deployment manages a set of identical pods and ensures that the desired number of replicas are running at all times. It is responsible for rolling out new versions of the application and rolling back to previous versions if necessary.

- StatefulSet: This type of workload is used for stateful applications that require unique network identifiers, stable storage, and ordered deployment. Examples of stateful applications include databases, queues, and key-value stores.

- DaemonSet: A DaemonSet ensures that a copy of a pod is running on every node in the cluster. This type of workload is used for background tasks that need to run on every node, such as monitoring agents or log collectors.

- Job: A job creates one or more pods and ensures that they run to completion. Jobs are used for batch processing and can be scheduled to run at specific times or on a recurring basis.

- CronJob: A CronJob is similar to a Job, but it is scheduled to run at specified intervals using a cron-like syntax. It is commonly used for periodic tasks such as backups or cleanup jobs.

By defining workloads in Kubernetes, you can take advantage of it's powerful scheduling, scaling, and self-healing capabilities. Kubernetes ensures that your workloads are running according to your desired specifications, and can automatically recover from failures and maintain high availability.

### Services
A Service is an abstraction that defines a logical set of Pods and also a policy by which to access them. Services allow you to define a single, stable IP address and DNS name for a set of Pods using selectors, even if those Pods are running on different nodes within the cluster.
I have created an example service manifest to show how they are structured and what the defined parameters are actually doing:

```yaml
apiVersion: v1
kind: Service           # 'apiVersion' and 'kind' define the type of Kubernetes object being created, in this case, a Service.
metadata:               # 'metadata' contains information about the Service, such as its name.
  name: my-service      
spec:                   # 'spec' defines the Service's specifications, including the 'selector', 'ports', and 'type'.
  selector:             # 'selector' specifies which Pods the Service will route traffic to. In this example, it selects Pods with the label 'app: my-app'.
    app: my-app
  ports:                # 'ports' define the ports on which the Service will listen for traffic. 
    - name: http        # In this example, the Service will listen on port '80' and forward traffic to port '8080' on the selected Pods.
      port: 80
      targetPort: 8080
  type: ClusterIP       # 'type' defines how the Service is exposed. It is set to 'ClusterIP', which means the Service is only accessible from within the cluster.

```

### Load Balancing
Load Balancing service is the process of distributing incoming network traffic across a selected group of Pods. This helps to improve the overall availability and scalability of your application, by ensuring that *all* traffic is evenly distributed across *all* available selected Pods.

To create a load balancing service manifest, you could simply use the example service manifest above as a template. Using that service manifest as a template you will need to update only 2 parameters! Give it a new 'name', so 'name: load-balance-service' and also change the 'type' from 'type: ClusterIP' to 'type: LoadBalancer'. 
My example:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: load-balance-service
spec:
  selector:
    app: my-app
  ports:
    - name: http
      port: 80
      targetPort: 8080
  type: LoadBalancer

```

### Networking
In Kubernetes, networking refers to the way in which different 'Pods' and 'Services' communicate with each other, both within a cluster and outside of it.

When you create a Kubernetes cluster, it creates a virtual network that allows different 'Pods' and 'Services' to communicate with each other, even if they are running on different 'Nodes' in the cluster. Each 'Pod' in the cluster gets its own IP address, which is used to route traffic to and from the 'Pod'.

Any other 'Pods' or 'Services' in the cluster can communicate with the 'Pod' by using the Service's IP address or DNS name. Kubernetes networking can become way more complex when you start to consider things like 'network policies', 'ingress controllers', and 'load balancers'. 

### Storage/Volumes
Storage and Volumes are used to persist data between container restarts and also to share data between different containers in a Pod. You can mount a local directory to make the data in your local directory available to your Kubernetes applications, without having to physically copy the data into a container image or upload it to an external cloud storage service.
Here's an example manifest of how to mount a local directory and make the data available within the pods containers:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-with-local-data
spec:
  containers:
    - name: my-app
      image: my-image:latest
      volumeMounts:
        - name: my-local-data
          mountPath: /app/data
  volumes:
    - name: my-local-data
      hostPath:
        path: /path/to/local/directory
```
The 'path' field in the 'hostPath' definition specifies the local directory that you want to mount. You will replace '/path/to/local/directory' with the path to the actual directory on your host machine.

The 'volumeMounts' field in the 'containers' definition specifies the name of the volume 'my-local-data' and the directory in which it should be mounted '/app/data' in this case.

WARNING! - This will allow the container to read and write data to the local directory on the host machine, which is now available as a Kubernetes volume.

This method of using 'hostPath' volumes can be LESS SECURE than using other types of volumes, as it grants access to the host machine's file system! Be careful when using 'hostPath' volumes, and ensure that you trust the source of the data that you are mounting.

### Cluster Administration
Kubernetes Cluster Administration involves managing the configuration, deployment, and maintenance of Kubernetes clusters. This can include managing nodes, monitoring resources, scaling applications, and ensuring the highest possible availability and security.

I will be using the Kubernetes command line tool [KubeCTL] for my standard/basic administration tasks. I will provide a few examples below of some simple administrative tasks likely to be performed using KubeCTL as i get started:

- Listing Nodes/Pods/Deployments/Services etc
`kubectl get <resource-type>` 
    This will display a list of all the <resource-type> you defined, in the cluster, along with their status, Age, IP address, and other details.

- Scaling a Deployment
`kubectl scale deployment/my-app --replicas=3`
    pretty self explanatory - This will scale the my-app deployment to have 3 replicas.

I will add more examples as I start to have a play about in my lab environment.