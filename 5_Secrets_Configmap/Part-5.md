# ConfigMaps and Secrets in Kubernetes🔒🔑🛡️

## What are ConfigMaps and Secrets in k8s ?
- In Kubernetes, **`ConfigMaps`** and **`Secrets`** are used to store configuration data and secrets, respectively. ConfigMaps store configuration `data as key-value pairs`, while Secrets `store sensitive data in an encrypted form`.

### Example:
- Imagine you're `in charge of a big spaceship (Kubernetes cluster)` with lots of different parts (containers) that need information to function properly.
- **ConfigMaps** are like a file cabinet where you `store all the information` each part needs in simple, labeled folders **(key-value pairs)**.
- **Secrets**, on the other hand, are like a safe where you keep the important, `sensitive information` that shouldn't be accessible to just anyone (encrypted data).
- So, using `ConfigMaps and Secrets`, you can ensure each part of your spaceship (Kubernetes cluster) has the information it needs to `work properly` and `keep sensitive information secure!`

**`Caution:`** **ConfigMap** does not `provide secrecy or encryption`. If the data you want to `store are confidential`, use a **Secret** rather than a ConfigMap, or use additional (third party) tools to keep your `data private`.

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
- **`Create a ConfigMap`** that stores the `database credentials` and `mount it as a volume` in the `deployment`.
**Let's create a mysql-config.yml file.**

```
apiVersion: v1
kind: ConfigMap
metadata:
  name: my-configmap
  labels:
    app: django-todo-app
  namespace: deploy1
data:
  MYSQL_DB: "database_todo"
```
- This yaml file has the `MYSQL_DB` which we will use in our `deployment.yml` file.
- We have also created a namespace `deploy1` for our deployment.

![Screenshot from 2023-06-11 13-14-50](https://github.com/Rohit312001/DevOps/assets/76991475/083f9de5-ddea-4289-80eb-9d6b781ecd89)

- Before lets create namespace `deploy1` using the command:
```
kubectl create namespace deploy1
```
![Screenshot from 2023-06-11 13-18-36](https://github.com/Rohit312001/DevOps/assets/76991475/5c55a66b-737a-4c72-b874-18bcbfccdd2f)

- Apply the updated deployment using the command: 

```
kubectl apply -f configmap.yml -n <namespace-name>
```

![Screenshot from 2023-06-11 13-19-21](https://github.com/Rohit312001/DevOps/assets/76991475/17499de3-52c5-405e-abec-7742e0dcbd1d)

- Verify that the ConfigMap has been created by checking the status of the ConfigMaps in your Namespace.

```
kubectl get configmaps -n <namespace-name>
```
![Screenshot from 2023-06-11 13-20-27](https://github.com/Rohit312001/DevOps/assets/76991475/062b5397-6fe8-4467-9a03-d5ac704ad187)

---

## Task 2:

- Before we create a secret, we need to create a `base64` encoded string of the `database password` that we will use in our `deployment.yml` file.
- I'm taking the following details as secret key:
**`Password`** : `Rohit@9936`

```
echo -n 'Rohit@9936' | base64
```

- For verifying the secret key, we can use the following command:
```
echo -n 'cm9oaXRAOTkzNg==' | base64 --decode
```

![Screenshot from 2023-06-11 13-17-39](https://github.com/Rohit312001/DevOps/assets/76991475/e5e63c62-6fcc-4e05-a244-90ecb664f71f)

- **`Create a Secret`** that stores the `database password` and `mount it as a volume` in the `deployment`.

```
apiVersion: v1
kind: Secret
metadata:   
  name: my-secret
  namespace: deploy1
type: Opaque
data:
  password: YWRtaW5AMTIz
```

![Screenshot from 2023-06-11 13-17-02](https://github.com/Rohit312001/DevOps/assets/76991475/3a44ba7e-dbfa-498a-a261-e20073a21483)

- This yaml file has the `password` which we will use in our `deployment.yml` file.
- We have also created a namespace `deploy1` for our deployment.

### what is Opaque type?
- Opaque type is used to store arbitrary data in secret object. 

- Apply the updated deployment using the command:
```
kubectl apply -f secret.yml -n <namespace-name>
```

![Screenshot from 2023-06-11 13-19-21](https://github.com/Rohit312001/DevOps/assets/76991475/b679874f-cac8-476f-89a3-0c277262f1bf)

- Verify that the Secret has been created by checking the status of the Secrets in your Namespace.
```
kubectl get secrets -n <namespace-name>
```

![Screenshot from 2023-06-11 13-25-05](https://github.com/Rohit312001/DevOps/assets/76991475/145fb7ea-49e4-45ad-9cbf-b0c3cfa1612d)

---

## Task 3:

- Now lets create a `deployment.yml` file for our deployment in which we add both `configmap` and `secret` in the deployment file.

```
 apiVersion: apps/v1
 kind: Deployment
 metadata:
   name: mysql-configuration
   labels:
     app: mysql
   namespace: deploy1 
 spec:
   replicas: 2
   selector:
     matchLabels:
       app: mysql
   template:
     metadata:
       labels:
         app: mysql
     spec:
       containers:
       - name: mysql-container
         image: mysql:8
         ports:
         - containerPort: 3306
         env:
         - name: MYSQL_ROOT_PASSWORD
           valueFrom:
             secretKeyRef:
               name: my-secret
               key: password
         - name: MYSQL_DATABASE
           valueFrom:
             configMapKeyRef:
               name: my-configmap
               key: MYSQL_DB
```

- In this yaml file we have added both `configmap` and `secret` in the deployment file.

![Screenshot from 2023-06-11 13-34-10](https://github.com/Rohit312001/DevOps/assets/76991475/8ed7343a-f05f-4f6e-a070-32efeafb275b)

- Apply the updated deployment using the command:
```
kubectl apply -f deployment.yml -n <namespace-name>
```
```
kubectl get pods -n <namespace>
```

![Screenshot from 2023-06-11 13-41-31](https://github.com/Rohit312001/DevOps/assets/76991475/5f869269-c7ca-4881-b8da-2ff50f750cd5)

---