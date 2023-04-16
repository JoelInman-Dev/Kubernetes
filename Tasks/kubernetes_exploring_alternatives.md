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

Docker Swarm is a container orchestration tool that is native to Docker, which enables users to manage and scale containerized applications across a cluster of Docker engine ['hosts']. It is simpler to set up and use than Kubernetes, making it a popular choice for small to medium-sized applications. It is also super lightweight with compatability typically consistent across operating systems and thus making it fairly simple to set up.

### Workloads

One of the key differences between Docker Swarm and Kubernetes is in the way they manage workloads. Docker Swarm uses a declarative approach to specify how applications should run on a cluster of hosts. This means that users define the desired state of the application, and Docker Swarm takes care of scheduling and scaling the containers to match that desired state.
In contrast, Docker Swarm offers automatic load balancing, while Kubernetes does not. However, with Docker Swarm there are no in-built monitoring mechanisms as standard, although, Docker Swarm supports monitoring through various third-party applications.

### Networking

Docker Swarm also provides a simpler networking model than Kubernetes. By default, Docker Swarm uses an overlay network that enables containers to communicate with each other. However, Docker Swarm lacks some of the advanced networking features that Kubernetes provides.

### Storage

In terms of storage, Docker Swarm has a more limited range of storage options than Kubernetes, but it still supports several types of storage drivers, including local, NFS, and Azure disk.

### Why use Docker Swarm vs Kubernetes

Overall, Docker Swarm is a simpler and easier-to-use container orchestration platform than Kubernetes, making it a popular choice for small to medium-sized applications that don't require the advanced features and scalability of Kubernetes.

If your applications are critical and you are looking to include monitoring, security features, high availability, and flexibility, then Kubernetes is the right choice.

[Docker Swarm | Documentation](https://docs.docker.com/engine/swarm/)

---

## Nomad

### What is Nomad

Nomad Like the others i have explored above, is a container orchestration tool developed by HashiCorp that enables users to manage and scale applications across a cluster of machines.

From what i have read so far, Nomad appears to be the happy medium area that offers the complexity options of Kubernetes coupled additionally with tools offering the simplicity of configuration that you get when utilising Apache Mesos or Docker Swarm. Nomad allows the orchastration of both containers and non-containerized applications.

Nomad is known for both its simplicity and overall flexibility, which makes it a popular choice for both small and large deployments.

### What is unique about Nomad Vs Kubernetes

Nomad is designed to be more lightweight and flexible than Kubernetes. While Kubernetes is a full-featured platform with a wide range of functionalities, Nomad aims to be simpler and more modular, allowing users to choose the components they need and leaving out the ones they don't.

### Managing workloads

Nomad uses a declarative model to manage workloads, similar to Kubernetes. Users define the desired state of the application, and Nomad takes care of scheduling and scaling the workload to match that desired state. Nomad supports a wide range of workload types, including Docker containers, Java applications, and more.

### Deployments

Nomad supports several deployment strategies, including canary, blue/green, and rolling deployments. It also allows for multiple versions of the same workload to coexist on the same cluster, making it easy to test new versions of an application before deploying them to production.

### Storage

Nomad supports a wide range of storage drivers, including local, network-attached storage, and cloud-based storage. It also includes a feature called "dynamic storage," which allows for storage to be allocated on demand as workloads are scheduled.

### Networking

Nomad includes built-in support for network segmentation and isolation, making it easy to secure applications and prevent unauthorized access. It also supports a range of network plugins, including Consul Connect and Calico.

### Why Choose Nomad over Kubernetes

Nomad is a good choice for organizations that prioritize flexibility and simplicity over the advanced features and scalability of Kubernetes. Its modular design and support for a wide range of workloads make it a good fit for organizations with diverse application needs. Nomad also has a smaller footprint than Kubernetes, which can be an advantage for organizations with limited resources or that are focused on running smaller workloads.

[Nomad | Documentation](https://developer.hashicorp.com/nomad/docs/)

---

## My Take from all of this

When it comes to choosing a cluster orchestration platform, it ultimately comes down to personal preference and the specific needs of the project. While Kubernetes is the most popular and widely used platform, it's important to consider other options. 

### I'm currently torn between Kubernetes and Nomad

Kubernetes definitely has its advantages - it offers better scalability and availability, has a huge community of developers, and offers a plethora of third-party tools and integrations. However, these advantages come with a lot of other complexities and a much steeper learning curve. For someone, like myself, who is just starting out with cluster orchestration, it can be overwhelming to dive into Kubernetes.

### On the other hand, Nomad offers a more modular approach - Which I like!

As a developer, you can add only the features that you want to use, making it a more lightweight and fully-featured option. It's also more straightforward to set up and use, making it a good option for smaller projects or those with limited resources. However, the adoption of Nomad is still relatively low, and it may not offer the same selection of employment opportunities and salaries as Kubernetes does.

Given these factors, it's understandable that I may feel more comfortable choosing Kubernetes for now. It's a well-established platform with a vast community and proven track record. However, as the industry continues to evolve, it's most definitely always worth revisiting other options and considering their benefits and drawbacks. As you gain more experience with cluster orchestration, you may find that Nomad **IS** a better fit for your specific needs and preferences.
