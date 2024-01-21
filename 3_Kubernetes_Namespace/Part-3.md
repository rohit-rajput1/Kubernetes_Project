# Namespaces in Kubernetes

## What are Namespaces ?
- In kubernetes, **`Namespaces`** are used to create `isolated environments` for resources. Each Namespace is like a **`separate cluster`** within the **`same physical cluster`**.

## Initial namespaces 
**`Four Namespaces already in Kubernetes (Predefined)`**:

**`default`**

- Kubernetes includes this namespace so that you can start using your new cluster without first creating a namespace.

**`kube-node-lease`**

- This namespace holds Lease objects associated with each node. Node leases allow the kubelet to send heartbeats so that the control plane can detect node failure.

**`kube-public`**
- This namespace is readable by all clients (including those not authenticated). This namespace is mostly reserved for cluster usage, in case that some resources should be visible and readable publicly throughout the whole cluster. The public aspect of this namespace is only a convention, not a requirement.

**`kube-system`**
- The namespace for objects created by the Kubernetes system.

**How to check namespaces in kubernetes cluster ?**
```
kubectl get namespace
```

![Screenshot from 2023-05-07 00-28-10](https://user-images.githubusercontent.com/76991475/236642054-7e99be52-994c-416c-9b85-1fccf5c62d38.png)

## Task 1:

### Create a Namespace for your Deployment.

![Screenshot from 2023-05-07 00-30-03](https://user-images.githubusercontent.com/76991475/236642062-4a7a213e-2cb6-4f5e-9f1e-32f4a8688184.png)

- To create a Namespace use the command
```
kubectl create namespace <namespace-name>
```

- Apply the updated deployment using the command:
```
kubectl apply -f deployment.yml -n <namespace-name>
```

![Screenshot from 2023-05-07 00-29-27](https://user-images.githubusercontent.com/76991475/236642059-8f9fa275-895d-4403-8fdc-1c91a116cfda.png)

Also, verified by using command:
```
kubectl get namespace
```

---
# Task 2:
## What are Services ?
- **`Services`** are used to expose your `Pods` and `Deployments` to the `network`.

![58-image-2](https://user-images.githubusercontent.com/76991475/236643088-4afefeea-761f-4afd-85f8-5e86bd696e8e.png)

- **Types of services** : `ClusterIP`, `NodePort`, `LoadBalancer`, and `ExternalName`.

![kubernates-services](https://user-images.githubusercontent.com/76991475/236643132-9b513882-3265-4c06-9d57-4265391db532.png)

**`ClusterIP`** 
- **ClusterIP** is the default type of service, which is used to expose a service on an IP address internal to the cluster.
- Access is only permitted from within the cluster.

**`NodePort`**
- **NodePorts** are open ports on every `cluster node`. **Kubernetes** will `route traffic` that comes into a `NodePort` to the service, even if the service is not running on that node. 
- NodePort is intended as a foundation for other `higher-level methods of ingress` such as `load balancers` and are useful in development.

**`LoadBalancer`** 
- For clusters running on public cloud providers like **AWS** or **Azure**, creating a load LoadBalancer service provides an equivalent to a clusterIP service, extending it to an external load balancer that is specific to the cloud provider. 
- Kubernetes will automatically create the **load balancer**, provide firewall rules if needed, and populate the service with the **external IP address assigned by the cloud provider**.

**`ExternalName`**  
- **`ExternalName`** services are similar to other Kubernetes services; however, instead of being accessed via a `clusterIP address`, it returns a `CNAME record` with a value that is defined in the externalName: parameter when creating the service.

---