# Project Overview


![Kubernetes](/2560px-Kubernetes_logo.svg.png)

### Table of Contents

[Part 1: Making of Kubernetes Cluster using Ngnix.](/1_Cluster_Making/Part-1.md)

1. **Kubernetes Cluster Setup:**
   Install Kubernetes using tools like kubeadm and create a cluster; then deploy Nginx pods by applying a YAML manifest specifying the desired replicas and container details.

2. **Service Exposure:**
   Expose the Nginx deployment with a Kubernetes service, allowing external access; apply the manifests using `kubectl apply` and verify the deployment's status.


[Part 2: Cluster_Deployment on AWS Cloud](/2_Cluster_Deployment/Part-2.md)

1. **AWS Infrastructure Setup:**
   Provision an Amazon EKS cluster or create EC2 instances for a self-managed Kubernetes cluster on AWS using tools like kops or eksctl.

2. **Nginx Deployment on AWS Cluster:**
   Deploy Nginx pods on the AWS Kubernetes cluster by applying YAML manifests, ensuring the correct configuration for AWS networking and permissions; expose the service to make Nginx accessible externally.


[Part 3: Kubernetes Namespace creation](/3_Kubernetes_Namespace/Part-3.md)

1. **Namespace Definition:**
   Create a Kubernetes namespace by defining a YAML manifest specifying the namespace's metadata, and apply it using `kubectl create`.

2. **Resource Allocation in Namespace:**
   Deploy and manage resources within the newly created namespace, ensuring isolation and organization of components within the Kubernetes cluster.


[Part 4: Kubernetes Service creation](/4_Kubernetes_Services/Part-4.md)

1. **Service Definition:**
   Define a Kubernetes service using a YAML manifest, specifying the target pods' selector labels, ports, and service type (e.g., ClusterIP, NodePort, LoadBalancer).

2. **Apply and Expose:**
   Apply the service manifest using `kubectl apply` to create the service, and expose it to make the associated pods accessible either within the cluster or externally, depending on the service type chosen.

[Part 5: Kubernetes Secrets & Configmap Creation](/5_Secrets_Configmap/Part-5.md)

1. **Secrets Creation:**
   Create Kubernetes secrets by defining a YAML manifest with sensitive data (e.g., passwords, API keys), encode them if necessary, and apply the manifest using `kubectl create`.

2. **ConfigMap Creation:**
   Define a ConfigMap in a YAML manifest with configuration data (e.g., environment variables, configuration files), apply it using `kubectl create`, and utilize it within pods for centralized configuration management.


[Part 6: Kubernetes Persistent Volume & Persistent Volume Claim Creation](/6_Kubernetes_Persistent_Volumes/Part-6.md)

1. **Persistent Volume (PV) Creation:**
   Define a Kubernetes Persistent Volume by specifying storage details such as capacity, access modes, and storage class in a YAML manifest; then apply the manifest using `kubectl create` to make the storage available to the cluster.

2. **Persistent Volume Claim (PVC) Creation:**
   Create a Persistent Volume Claim by defining a YAML manifest with storage requirements matching those of the desired PV, and apply it using `kubectl create` to bind the claim to an available PV, providing dynamic storage allocation for pods.

---
