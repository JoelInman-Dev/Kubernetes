# Recreate, Rolling, Blue/Green, Canary & A/B Testing

In Kubernetes, there are several ways to deploy applications to a cluster, each with its own deployment strategy.

I will be looking at some of the most common types of deployment strategies in Kubernetes; In an effort to understand Deployments more deeply and their specific strategy use cases.

---

## Recreate Deployment

This is a deployment strategy that replaces the entire old application version with a new one entirely. This is done by creating a new set of replicas with the updated application version and deleting the old set.

For example, if you have a web application running with 3 replicas, the recreate deployment strategy will create 3 new replicas with the updated application version and delete the previous 3 replicas.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp
spec:
  replicas: 1
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
        - name: myapp
          #image: myapp:v1
          image: myapp:v2
          ports:
            - containerPort: 80

```

This ensures that the application is not partially updated, but fully replaced.

However, the Recreate Deployment strategy can also be riskier since it involves downtime while the deployment is being recreated. Additionally, any changes to the deployment or its configuration will affect all users at once, which can increase the risk of errors or disruptions.

---

## Rolling Updates

This deployment strategy involves gradually updating an existing application by replacing a few instances of the old version with instances of the new version until all instances have been updated. Rolling updates ensure that the application remains available throughout the deployment process.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-app
        image: my-image:v2
        ports:
        - containerPort: 80
```

---

## Blue/Green Deployment

This deployment strategy involves deploying two identical environments, one with the current version of the application (blue) and the other with the new version (green). Traffic is then switched from the blue environment to the green environment - only once the new version has been tested and validated.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-app
spec:
  selector:
    app: my-app-green
  ports:
  - name: http
    port: 80
    targetPort: http
  type: LoadBalancer
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app-blue
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app-blue
  template:
    metadata:
      labels:
        app: my-app-blue
    spec:
      containers:
      - name: my-app
        image: my-image:v1
        ports:
        - containerPort: 80
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app-green
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app-green
  template:
    metadata:
      labels:
        app: my-app-green
    spec:
      containers:
      - name: my-app
        image: my-image:v2
        ports:
        - containerPort: 80
```

---

## Canary Deployment

This deployment strategy involves gradually rolling out a new version of the application, but only to a small percentage of users while keeping the old version running for the rest of the users. This allows for testing and validation of the new version before rolling it out to all users.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-app
        image: my-image:v1
        ports:
        - containerPort: 80
---
apiVersion: networking.k8s.io/v1beta1
kind: Ingress
metadata:
  name: my-app
  annotations:
    nginx.ingress.kubernetes.io/canary: "true"
spec:
  rules:
  - host: example.com
    http:
      paths:
      - path: /
        backend:
          serviceName: my-app
          servicePort: 80
      - path: /canary
        backend:
          serviceName: my-app-canary
          servicePort: 80
---
apiVersion: apps/v1
kind: Deployment
metadata

```

---

## A/B Testing Deployment

This is a deployment strategy that deploys two versions of the same application simultaneously, and then routes traffic to each version based on defined rules.

This is done by creating two sets of replicas, one with the old application version and the other with the new version. 

Traffic is then routed to each version based on defined rules. For example, you might send 80% of traffic to the old version and 20% to the new version, gradually increasing the percentage to test the new version's performance. 

This allows you to test new features or changes with a small percentage of users before rolling out the update to everyone.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp
spec:
  replicas: 6
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
        version: "v1"
    spec:
      containers:
      - name: myapp
        image: myapp:v1
        ports:
        - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: myapp
spec:
  selector:
    app: myapp
  ports:
  - name: http
    protocol: TCP
    port: 80
    targetPort: 80
  type: LoadBalancer
```

In this manifest, the deployment creates six replicas of the "myapp" application, which listens on port 80. The deployment specifies that the initial version of the application is "v1". The service exposes the deployment to external traffic on port 80.

To perform an A/B test, you can create a new deployment that specifies a different version of the application.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp-v2
spec:
  replicas: 6
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
        version: "v2"
    spec:
      containers:
      - name: myapp
        image: myapp:v2
        ports:
        - containerPort: 80
```

This one creates a new deployment with six replicas of the "myapp" application, **version "v2"**. 

To direct a percentage of traffic to this new version, you can update the service configuration as follows:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: myapp
spec:
  selector:
    app: myapp
  ports:
    - name: http
      port: 80
      targetPort: http
  type: LoadBalancer
  sessionAffinity: None
  loadBalancerSourceRanges:
    - 0.0.0.0/0
  externalTrafficPolicy: Cluster
  publishNotReadyAddresses: true
  ---
  apiVersion: networking.k8s.io/v1beta1
  kind: Ingress
  metadata:
    name: myapp
  spec:
    rules:
      - host: myapp.example.com
        http:
          paths:
            - path: /
              backend:
                serviceName: myapp
                servicePort: http
    annotations:
      nginx.ingress.kubernetes.io/canary: "true"
      nginx.ingress.kubernetes.io/canary-weight: "50"
```

The `nginx.ingress.kubernetes.io/canary` and nginx.`ingress.kubernetes.io/canary-weight` annotations are used to direct 50% of the traffic to the new version of the app. The remaining 50% of traffic is still directed to the original version.

Overall, the A/B testing deployment strategy can help you gradually introduce new features or changes to your application while minimizing the risk of downtime or disruption for your users.

---

## The Pros & Cons of Each Strategy

### Recreate Deployment Strategy

    Pros:
        Simple and easy to understand.
        Good for stateless applications.

    Cons:
        Downtime during deployment.
        Not suitable for stateful applications.

### Rolling Update Deployment Strategy

    Pros:
        Zero downtime deployment.
        Better for stateful applications.

    Cons:
        Not suitable for applications that require specific order of updates.
        Can cause performance degradation during update.

### Blue-Green Deployment Strategy

    Pros:
        Zero downtime deployment.
        Easy rollback to the previous version.

    Cons:
        Requires additional infrastructure resources.
        Longer deployment time due to provisioning of new infrastructure.

### Canary Deployment Strategy

    Pros:
        Low risk deployment.
        Provides early feedback on new version.

    Cons:
        Requires careful monitoring and analysis.
        Requires additional infrastructure resources.

### A/B Testing Deployment Strategy

    Pros:
        Allows for testing of multiple versions simultaneously.
        Provides data-driven decision making.

    Cons:
        Requires additional infrastructure resources.
        Can lead to confusion among users.

## Take Away

### *It is important to choose the deployment strategy that best suits your application requirements and constraints*

---
For example, if you have a stateful application that cannot tolerate downtime, a Rolling Update Deployment Strategy might be the best choice

On the other hand, if you have a complex application that requires rigorous testing and validation, a Canary Deployment Strategy might be a better fit
