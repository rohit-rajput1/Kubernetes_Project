# Services in Kubernetes

### What is Service in Kubernetes?
- A **Kubernetes Service** is an `abstraction` which defines a logical set of Pods running somewhere in your cluster, that all provide the same functionality. When created, each Service is assigned a **`unique IP address (also called clusterIP).`** 
- This `address` is tied to the `lifespan of the Service`, and will not change while the Service is alive. Pods can be configured to talk to the Service, and know that communication to the Service will be automatically load-balanced out to some pod that is a member of the Service.

---
### Types of Services in Kubernetes:

![6403ec82472efb1d6ff79f4e_d](https://github.com/Rohit312001/90DaysOfDevOps/assets/76991475/2fffd76e-1ee0-4fb3-b6ba-7f3fa33e6e3e)

- **`Visual Representation of Services in Kubernetes:`**

![1_tnK94zrEwyNe1hL-PhJXOA](https://github.com/Rohit312001/90DaysOfDevOps/assets/76991475/bec9683a-08b9-45ac-9df0-aafb840aeded)

---

## So before starting the task lets do necessary installation and setup.

- **Before Doing the task let's set up the git repository for the same and push the code to the repository.**

**`Steps to follow:`**
Here we will use AWS for Deployment and Git for version control.

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

**`Step-09`**: Now Clone the repository from the github.
Github Repo link : https://github.com/LondheShubham153/django-todo-cicd.git

```
git clone https://github.com/LondheShubham153/django-todo-cicd.git
```

**`Step-10`**: Now lets check the files in the repository.
``` 
ls
cd django-todo-cicd
```
![Screenshot from 2023-05-05 16-03-41](https://user-images.githubusercontent.com/76991475/236487906-65ba91a5-36ac-407d-a214-0b42d162c9ea.png)

---
## Task-1: 

- **Create a Service for the deployment.**

**`Step-01`**: Create a file **`service.yaml`** and write the code for the service.

```
apiVersion: v1
kind: Service
metadata:
  name: django-todo-services
  namespace: python-django-app
spec:
  type: NodePort
  selector:
    app: django-app
  ports:
  - protocol: TCP
    port: 8000
    targetPort: 30007
```

![Screenshot from 2023-06-02 23-35-16](https://github.com/Rohit312001/90DaysOfDevOps/assets/76991475/05c0574c-adc3-4615-bfc8-7cc76c3bea5f)

- **Now lets create a namespace first for the deployment.**
```
kubectl create namespace python-django-app
```

![Screenshot from 2023-06-02 23-35-52](https://github.com/Rohit312001/90DaysOfDevOps/assets/76991475/aad02940-2196-4543-bcf0-4b7ed05b172b)

![Screenshot from 2023-06-02 23-36-14](https://github.com/Rohit312001/90DaysOfDevOps/assets/76991475/207da267-404a-45c0-ba19-f79c554fcbbf)

- **As we have created services and namespace now lets create a deployment.**

**Syntax:** ```kubectl create -f <filename> -n <namespace>```

```
kubectl create -f service.yaml -n python-django-app
```

![Screenshot from 2023-06-02 23-36-24](https://github.com/Rohit312001/90DaysOfDevOps/assets/76991475/3c102133-7927-4c52-9776-156fb2e43bc2)

![Screenshot from 2023-06-02 23-36-59](https://github.com/Rohit312001/90DaysOfDevOps/assets/76991475/e1d84ad8-9104-4c77-be0b-fe2157062e1d)

- **Now verify the deployment and service.**

```
kubectl get all -n python-django-app
```

---

## Task-2:

- **Create a ClusterIP service for the deployment.**

**`Step-01`**: Create a file **`clusterip-service.yaml`** and write the code for the service.

```
apiVersion: v1
kind: Service
metadata:
  name: django-todo-services
  namespace: python-django-app
spec:
  type: NodePort
  selector:
    app: django-app
  ports:
  - protocol: TCP
    port: 8000
    targetPort: 8000
  type: ClusterIP  
```

![Screenshot from 2023-06-02 23-53-54](https://github.com/Rohit312001/90DaysOfDevOps/assets/76991475/d43ab63a-b8d7-4a68-9928-de1a390a7e09)

- **Now lets create a deployment.**

**Syntax:** ```kubectl create -f <filename> -n <namespace>```

```
kubectl create -f clusterip-service.yaml -n python-django-app
```

![Screenshot from 2023-06-02 23-54-36](https://github.com/Rohit312001/90DaysOfDevOps/assets/76991475/228c833d-b605-4bb4-af67-8e6ef4b07a97)

- **Check of Service is created or not.**

```
kubectl get svc -n python-django-app
```

![Screenshot from 2023-06-02 23-55-46](https://github.com/Rohit312001/90DaysOfDevOps/assets/76991475/07be8dff-5cb9-49a2-87a7-2c7cd90aa749)

- **Now verify the deployment and service.**

```
kubectl get all -n python-django-app
```

---

## Task-3:

- **Create a LoadBalancer service for the deployment.**

**`step-1`**: Create a file **`loadbalancer-service.yaml`** and write the code for the service.

```
apiVersion: v1
kind: Service
metadata:
  name: django-todo-lb-services
  namespace: python-django-app
spec:
  type: NodePort
  selector:
    app: django-app
  ports:
  - protocol: TCP
    port: 8000
    targetPort: 8000
  type: LoadBalancer 
```

![Screenshot from 2023-06-03 00-13-53](https://github.com/Rohit312001/90DaysOfDevOps/assets/76991475/49da4132-157d-4535-bf76-bd683dfb9d92)

- **Now lets create a deployment.**
**`syntax:`** ```kubectl create -f <filename> -n <namespace>```

```
kubectl create -f loadbalancer-service.yaml -n python-django-app
```

![Screenshot from 2023-06-03 00-15-07](https://github.com/Rohit312001/90DaysOfDevOps/assets/76991475/6c64aabd-cc68-4261-b72a-00a6680c0c3e)

- **Check of Service is created or not.**

```
kubectl get svc -n python-django-app
minikube service list
```
![Screenshot from 2023-06-03 00-16-02](https://github.com/Rohit312001/90DaysOfDevOps/assets/76991475/b21ad42c-5122-474c-b4a9-fc882152e977)

- **Now verify the deployment and service.**

```
kubectl get all -n python-django-app
```

---