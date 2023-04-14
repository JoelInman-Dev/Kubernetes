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
will learn more in depth before contributing my thoughts...

### Load Balancing
will learn more in depth before contributing my thoughts...

### Networking
will learn more in depth before contributing my thoughts...

### Storage/Volumes
will learn more in depth before contributing my thoughts...

### Configuration
will learn more in depth before contributing my thoughts...

### Cluster Administration
will learn more in depth before contributing my thoughts...