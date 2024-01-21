# Kubernetes Cluster with Nginx.

## Kubernetes Overview

- Kubernetes is a **portable**, **extensible**, **open source platform** for managing `containerized workloads` and `services`, that facilitates both **declarative configuration** and **automation**. 
- It has a large, rapidly growing ecosystem. Kubernetes services, support, and tools are widely available.
- **Google** open-sourced the **Kubernetes project in 2014**.Kubernetes combines over 15 years of Google's experience running production workloads at scale with best-of-breed ideas and practices from the community.

### What is Kubernetes?

- **`Kubernetes`** is a platform for managing containerized applications at scale. It automates **deployment**, **scaling**, and **management of applications** using a declarative approach and provides features like **self-healing**, **automatic scaling**, and **rolling updates**.
- It is built on a cluster architecture and offers a wide range of plugins and extensions for integration with other tools and services.

---

## What is Minikube?

- **Minikube** is a tool that makes it easy to run **Kubernetes locally**. Minikube runs a `single-node Kubernetes cluster` inside a **`Virtual Machine (VM)`** on your laptop for users looking to try out Kubernetes or develop with it day-to-day.
- Minikube is available for **Windows**, **macOS**, and **Linux**.

## Features of Minikube:

- Supports the latest Kubernetes release (+6 previous minor versions).
- Cross-platform `(Linux, macOS, Windows)`.
- Deploy as a `VM`, `a container`, or on `bare-metal`.
- **Multiple container runtimes** (CRI-O, containerd, docker).
- Direct `API` endpoint for blazing fast image load and build.
- Advanced features such as **LoadBalancer**, **filesystem mounts**, **FeatureGates**, and **network policy**.
- Addons for easily installed Kubernetes applications.
- Supports common `CI environments`.

---

## Task-01:

### Install minikube on your local machine (Here I have used AWS EC2 instance)

**`Step-01`**: Open your terminal or any other cloud service.

**`Step-02`**: Update and install **`Docker`** on your machine.
```
sudo apt update
sudo apt install docker.io
```
![Screenshot from 2023-04-17 22-08-47](https://user-images.githubusercontent.com/76991475/232585569-3cbee90e-4170-402c-8111-a72ad4c7bdcc.png)

![Screenshot from 2023-04-17 22-15-44](https://user-images.githubusercontent.com/76991475/232585601-cd2a017f-ed21-4b64-9140-17511bfdeba6.png)

**`Step-03`**: Now give Docker permission for supder user of your local machine.
```
sudo usermod -aG docker $USER && newgrp docker
```

![Screenshot from 2023-04-17 22-24-25](https://user-images.githubusercontent.com/76991475/232585725-7d9e0ffa-22fd-4c41-b1ca-b2fb042806f4.png)

**`Step-04`**: Now lets install **`Minikube`**.
```
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64

sudo install minikube-linux-amd64 /usr/local/bin/minikube
```

![Screenshot from 2023-04-17 22-19-41](https://user-images.githubusercontent.com/76991475/232585632-70ccfaf5-8161-48b4-acf9-bcbccead3a7b.png)

![Screenshot from 2023-04-17 22-20-07](https://user-images.githubusercontent.com/76991475/232585648-7bc42ab1-6b46-4bfb-863c-bc32b495fab9.png)

![Screenshot from 2023-04-17 22-20-36](https://user-images.githubusercontent.com/76991475/232585668-c3813b2b-fc9e-4cee-823f-44d485172a04.png)

**`Step-05`**: Now lets start **`Minikube`**.
```
minikube start
```

- **lets connect with docker.**
```
minikube start — driver=docker
```
![Screenshot from 2023-04-17 22-22-18](https://user-images.githubusercontent.com/76991475/232585683-ec343849-43d3-4e71-8981-9725b42120c0.png)

![Screenshot from 2023-04-17 23-06-54](https://user-images.githubusercontent.com/76991475/232587586-a3d91674-f519-4eb8-ba71-8b2f6a3496d3.png)

**`Step-06`**: Now lets check the status of **`Minikube`**.
```
minikube status
```

**`Step-07`**: Now we will install the Command line instruction for Minikube which is **`Kubectl`**.

- **But first install snap on your machine.**
```
sudo apt install snap
```
![Screenshot from 2023-04-17 23-10-55](https://user-images.githubusercontent.com/76991475/232585748-700042ea-fff4-4a6f-884c-b5dfe2ed92b8.png)

- **CLI for Minikube is Kubectl.**
```
sudo snap install kubectl --classic
```

![Screenshot from 2023-04-17 23-12-11](https://user-images.githubusercontent.com/76991475/232585758-3bf76427-a388-478e-94c1-c00ee5b18a75.png)

**`Step-08`**: Now lets check the version of **`Kubectl`**.
```
kubectl version
```

---

## Let's understand the concept pod.

- **Pods** are the smallest deployable units of computing that you can create and manage in `Kubernetes`.
- A Pod `(as in a pod of whales or pea pod)` is a group of one or more containers, with **shared storage** and **network resources**, and a specification for how to run the containers.
- A Pod's contents are always **co-located** and **co-scheduled**, and run in a `shared context`. A Pod models an application-specific "logical host": it contains one or more application containers which are relatively tightly coupled. 
- In non-cloud contexts, applications executed on the same **physical or virtual machine** are analogous to cloud applications executed on the same logical host.

# Task-02:

## Create your first pod on Kubernetes through minikube.

- **Lets create a folder for pod.**
```
mkdir Minikube_Project

cd Minikube_Project
```

![Screenshot from 2023-04-17 23-33-31](https://user-images.githubusercontent.com/76991475/232585768-5c734dd5-c5bf-4d92-ae16-d5cce7d1a295.png)

- Let's create a namespace for Nginx pod.
```
kubectl create namespace nginx

kubectl get namespaces
```

![Screenshot from 2023-04-17 23-35-52](https://user-images.githubusercontent.com/76991475/232585779-a64738e1-c8fd-4751-85ab-fb44300b3588.png)

- **Now lets  write a yaml file for pod.**
```
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx:1.14.2
    ports:
    - containerPort: 80
```

![Screenshot from 2023-04-17 23-37-45](https://user-images.githubusercontent.com/76991475/232585792-6df2c2e8-2517-4881-877c-201eb06ac702.png)

- **As we have wrote yaml file now we will configure it, applying it**
```
kubectl apply -f pod.yaml
```

![Screenshot from 2023-04-17 23-38-56](https://user-images.githubusercontent.com/76991475/232585801-8a1ddc58-f153-48fe-a0aa-9fc05253cf99.png)

- **Check pod is created or not using command.**
```
kubectl get pods
```
![Screenshot from 2023-04-17 23-40-51](https://user-images.githubusercontent.com/76991475/232585807-c7802ffc-9978-4e29-9f98-be824556c116.png)

---

