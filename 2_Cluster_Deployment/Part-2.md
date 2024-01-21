# Launching your Kubernetes Cluster with Deployment

## What is Deployment in k8s ?

- A **`Deployment`** provides a configuration for updates for `Pods` and `ReplicaSets`.
- You describe a desired state in a **Deployment**, and the Deployment Controller changes the actual state to the desired state at a controlled rate. You can define Deployments to create new replicas for scaling, or to remove existing Deployments and adopt all their resources with new Deployments.

---
## Before Doing the task let's set up the git repository for the same and push the code to the repository.

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

**`Step-11`**: Now lets create a **`Dockerfile`**.
```
vim Dockerfile
```

![Screenshot from 2023-05-05 16-13-54](https://user-images.githubusercontent.com/76991475/236487916-93c7e27d-7e17-46d4-81ca-0817270a2aa8.png)

**`Step-12`**: Now lets build the image.
```
docker build -t rohit8329/django-todo:latest .
```

![Screenshot from 2023-05-05 16-14-07](https://user-images.githubusercontent.com/76991475/236487927-f56c3f78-f636-4761-9418-ca5f2ceffd8f.png)

**`Step-13`**: Now lets create a docker container for checking the image is running on port or not.
```
docker run -d -p 8000:8000 rohit8329/django-todo:latest
```

![Screenshot from 2023-05-05 16-24-00](https://user-images.githubusercontent.com/76991475/236487956-79b529d2-aac0-49bf-ba57-0bcff62958f5.png)

**`Step-14`**: Now we will push the docker image to the docker hub.
```
docker login
docker push rohit8329/django-todo:latest
```

![Screenshot from 2023-05-05 16-57-32](https://user-images.githubusercontent.com/76991475/236503478-79bf63c0-0b6d-410e-b7b0-64c921f8dccd.png)

---
# Task-1: 

### Create one Deployment file to deploy a sample todo-app on K8s using "Auto-healing" and "Auto-Scaling" feature

### Now let's create Kubernetes pod using YAML file.

```
apiVersion: v1

kind: Pod

metadata:

  name: todo-pod

spec:

  containers:

  - name: todo-app

    image: rohit8329/django-todo:latest

    ports:

    - containerPort: 8000
```

![Screenshot from 2023-05-05 18-42-13](https://user-images.githubusercontent.com/76991475/236503496-2228e060-e063-4383-9ebf-8208dbda0545.png)

![Screenshot from 2023-05-05 18-42-47](https://user-images.githubusercontent.com/76991475/236503504-54c3df1a-171e-42a1-a861-8942e0a083a8.png)

### Now let's apply pod YAML file.

```
kubectl apply -f todo-pod.yaml
```

![Screenshot from 2023-05-05 19-00-50](https://user-images.githubusercontent.com/76991475/236503511-27d571eb-74cc-4021-a055-99b7ee26b3a1.png)

### Concept:

**`Auto-Healing`** : **Auto-healing** is a feature that allows `Kubernetes to automatically restart containers` that **fail** for various reasons. It is a very useful feature that helps to `keep your applications up and running`.

**`Auto-Scaling`** : **Auto-scaling** is a feature that allows `Kubernetes to automatically scale the number of pods` in a deployment based on the resource usage of the existing pods.

![Screenshot from 2023-05-05 19-53-08](https://user-images.githubusercontent.com/76991475/236506510-793f4012-4add-49ab-aa19-332f1a4c776a.png)

Here on purpose I have killed the `pod` to check the **`auto-healing`** feature.
for **`Auto scaling`** we use `replica set`.

---