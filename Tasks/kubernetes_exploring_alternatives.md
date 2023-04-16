# Kubernetes is not the ONLY Container Orchestration Platform

**Despite Kubernetes' popularity, it's not always the best choice compared with rival orchestrators like Apache Mesos.**

*Rather than viewing Kubernetes as overall better or worse than alternatives, it's more useful to understand the fundamental differences in how each platform operates.*

---

## Apache Mesos

Apache Mesos is an open-source platform that manages clusters of servers. It was introduced in 2009, before the widespread adoption of containers after Docker's launch in 2013.

Mesos takes a user-defined workload, called a task, and runs it on a cluster of servers. It can run not only containers but also non-containerized applications.

Although Kubernetes is currently the most popular orchestration platform, it initially faced competition from other container orchestrators such as Docker Swarm and Mesos.

### Deploying workloads

In Kubernetes, the most common way to deploy a workload after defining it is to use the following kubectl command:

`kubectl apply -f your-deployment.yaml`

In contrast, Mesos users can deploy workloads with API calls using generic Linux commands like curl instead of an external tool like kubectl.

`curl -X POST http://localhost:8080/v2/apps -d @hello-marathon.json -H "Content-type: application/json"`

This small convenience eliminates the need to install a specialized tool on every machine to administer a Mesos cluster.

### Networking

Mesos has a significantly simpler networking setup compared to Kubernetes, however it's less adaptable.

Kubernetes has networking management plugins that are compatible with the Container Network Interface (CNI). Depending on the complexity of your networking needs, you can choose from various Kubernetes networking plugins.

Mesos doesn't use networking plugins by default. It has built-in networking features that can assign ports to workloads, but it doesn't provide detailed control over network configuration. Although Mesos has CNI support, it's not as developed, and not all CNI plugins are compatible with Mesos.

### Storage

Similar to networking, Mesos is easier to configure but less adaptable than Kubernetes on the storage front.

In general, production workloads on Mesos are restricted to using local storage volumes to persist data. Mesos workloads **can** access persistent storage resources hosted on nodes in clusters, similar to using local persistent volumes in Kubernetes.

### Reasons to choose Mesos vs. Kubernetes

Why would an organization choose Mesos over Kubernetes?

Mesos has been around longer and is more established. It can also be simpler to set up if it meets your networking and storage needs using its built-in features. Mesos can run non-containerized apps natively, which is an advantage for organizations deploying various application types.

However, Kubernetes is a better option for organizations seeking to benefit from hundreds of open-source add-ons and integrations provided by the Kubernetes community.

[Apache Mesos | Documentation](https://mesos.apache.org/documentation/latest/)

---

## Docker Swarm

Docker Swarm is a container orchestration tool that enables users to manage and scale containerized applications across a cluster of Docker hosts. It is simpler to set up and use than Kubernetes, making it a popular choice for small to medium-sized applications.

### Workloads

One of the key differences between Docker Swarm and Kubernetes is in the way they manage workloads. Docker Swarm uses a declarative approach to specify how applications should run on a cluster of hosts. This means that users define the desired state of the application, and Docker Swarm takes care of scheduling and scaling the containers to match that desired state.

### Networking

Docker Swarm also provides a simpler networking model than Kubernetes. By default, Docker Swarm uses an overlay network that enables containers to communicate with each other. However, Docker Swarm lacks some of the advanced networking features that Kubernetes provides.

### Storage

In terms of storage, Docker Swarm has a more limited range of storage options than Kubernetes, but it still supports several types of storage drivers, including local, NFS, and Azure disk.

### Why use Docker Swarm vs Kubernetes

Overall, Docker Swarm is a simpler and easier-to-use container orchestration platform than Kubernetes, making it a popular choice for small to medium-sized applications that don't require the advanced features and scalability of Kubernetes.

---

## Nomad
