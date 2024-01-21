# Persistent Volumes Deployment in Kubernetes.

## What are Persistent Volumes in k8s ?
- A **`PersistentVolume (PV)`** is a piece of `storage in the cluster `that has been provisioned by an administrator or dynamically provisioned using Storage Classes. 
- It is a resource in the cluster just like a node is a cluster resource. `PVs are volume` plugins like `Volumes`, but have a lifecycle independent of any individual Pod that uses the PV. 
- This API object captures the details of the implementation of the storage, be that NFS, iSCSI, or a cloud-provider-specific storage system.

## What are Persistent Volume Claims in k8s ?
- A **`PersistentVolumeClaim (PVC)`** is a request for storage by a user. It is similar to a Pod. Pods consume node resources and PVCs consume PV resources. Pods can request specific levels of resources (CPU and Memory). Claims can request specific size and access modes (e.g., they can be mounted `ReadWriteOnce`, `ReadOnlyMany` or `ReadWriteMany`, see **`AccessModes`**.

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

## Task 1:

### Create a Persistent Volume of 1Gi.

- Create a file **`pv.yaml`** and write the code for Persistent Volume.
```
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-django-todo-app
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Retain
  hostPath:
    path: "/mnt/data"
```

![Screenshot from 2023-06-16 23-55-57](https://github.com/Rohit312001/GitDemo/assets/76991475/7d52b696-4af8-4d40-bdc0-ad79066a0df7)

```
kubectl apply -f pv.yaml
```

![Screenshot from 2023-06-16 23-41-05](https://github.com/Rohit312001/GitDemo/assets/76991475/23732325-0a54-4d2f-a75c-b70c941d0bed)

- Create a file **`pvc.yaml`** and write the code for Persistent Volume Claim.
```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc-django-todo-app
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 500Mi
```

![Screenshot from 2023-06-16 23-56-27](https://github.com/Rohit312001/GitDemo/assets/76991475/ab420b54-b239-414c-861f-ce3d0a56a18b)

```
kubectl apply -f pvc.yaml
```

![Screenshot from 2023-06-16 23-41-33](https://github.com/Rohit312001/GitDemo/assets/76991475/5fe1273d-71a2-4ff3-a1d6-6a650f64b501)

- Create a file **`deployment.yaml`** and write the code for Deployment.

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: django-todo-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: django-todo-app
  template:
    metadata:
      labels:
        app: django-todo-app
    spec:
      containers:
        - name: django-todo-app
          image: rohit8329/django-todo:latest
          ports:
            - containerPort: 8001
          volumeMounts:
            - name: django-todo-app-data
              mountPath: /tmp/app
      volumes:
        - name: django-todo-app-data
          persistentVolumeClaim:
            claimName: pvc-django-todo-app
```

![Screenshot from 2023-06-16 23-56-54](https://github.com/Rohit312001/GitDemo/assets/76991475/4d06d3b2-cc6d-4546-a741-b57cce98ebbc)

```
kubectl apply -f deployment.yaml
```

![Screenshot from 2023-06-16 23-42-11](https://github.com/Rohit312001/GitDemo/assets/76991475/7c6dc29f-db15-42e5-9516-471c4577daa0)

- Verify that the Persistent Volume has been added to your Deployment by checking the status of the Pods and Persistent Volumes in your cluster. Use this commands.

```
kubectl get pods
```

![Screenshot from 2023-06-16 22-02-06](https://github.com/Rohit312001/GitDemo/assets/76991475/d398cded-c58b-4134-8a33-1508e0f9cfb5)

---

## Task 2:

- Connect to a Pod in your Deployment using command :

![Screenshot from 2023-06-16 23-42-43](https://github.com/Rohit312001/GitDemo/assets/76991475/ed3c51f3-b541-460a-ad92-ac977a29ba62)

```
kubectl exec -it <pod-name> -- /bin/bash
```
- Here we can create file in the pod and check the data in the Persistent Volume in the interactive shell.

```
cd /tmp/app
```

- Create a file **`test.txt`** and write some data in it.

```
echo "Hello World" > test.txt
```

- At last exit from the pod.

- Verify that you can access the data stored in the Persistent Volume from within the Pod by checking the contents of the file you created in the Pod.

---


