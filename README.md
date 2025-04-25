# 🚀 Helm Chart Customization

This project demonstrates how to deploy a simple web application to a Kubernetes cluster using **Helm**, a powerful package manager for Kubernetes. It includes the setup of Helm, customization of a Helm chart, and deployment of the application.

---


## 📦 Project Overview

This project helps you:

- Understand Helm charts, values, and templates.
- Customize a Helm chart to deploy Nginx.
- Set resource requests and limits.
- Install and verify your deployment in Kubernetes.

---

## 🧰 Prerequisites

Make sure you have the following installed:

- [Helm](https://helm.sh/docs/intro/install/)
- [kubectl](https://kubernetes.io/docs/tasks/tools/)
- A running Kubernetes cluster (like Minikube, Docker Desktop, or a cloud-based cluster)
- Git
- VS Code or any text editor

--- 

## 📁 Project Structure
```
webapp/ ├── Chart.yaml ├── values.yaml └── templates/ └── deployment.yaml
```

---


## Verify installation:
```
helm version
```
![](./img/1b.version.png)

---

## : Create a new project directory:
```
mkdir helm-web-app
cd helm-web-app
```
![](./img/2a.mkdir.cd.helm.png)

---

## Step 1: 🛠️ Create a Helm Chart
```
helm create webapp
```
![](./img/2b.create,web.png)

---


## 1.2 : Modify values.yaml
Edit **values.yaml** and update the following values:
```
replicaCount: 2

image:
  repository: nginx
  tag: stable
  pullPolicy: IfNotPresent
```


## 1.3: Customize the Deployment Template
Open templates/deployment.yaml and find this line:
```
{{- toYaml .Values.resources | nindent 12 }}
```
Remove it, and replace it with this:
```
resources:
  requests:
    memory: "128Mi"
    cpu: "100m"
  limits:
    memory: "256Mi"
    cpu: "200m"
```
**This sets the resource usage for your app.**


### Create a New GitHub Repository
```
helm-web-app-2
```

## 1.4: Initialize Git:
```
git init
git add .gitignore
git add .
git commit -m "Initial Helm chart"
```

## Push to remote repository:
```
git remote add origin <REMOTE_REPOSITORY_URL>
git push -u origin master
```

---


## Step 2: Install the Chart in Kubernetes
Make sure you're in the root directory (helm-web-app), then run:
```
minikube start --driver=virtualbox
minikube status
helm install my-webapp ./webapp
```
![](./img/3a.helm.install.png)


## Verify the Deployment
```
kubectl get deployments
```
**You should see my-webapp running with 2 replicas.**
![](./img/3b.kube.get.png)



## Get the Application URL

Run the following commands to find pod and port info:
```
export POD_NAME=$(kubectl get pods --namespace default -l "app.kubernetes.io/name=webapp,app.kubernetes.io/instance=my-webapp" -o jsonpath="{.items[0].metadata.name}")

export CONTAINER_PORT=$(kubectl get pod --namespace default $POD_NAME -o jsonpath="{.spec.containers[0].ports[0].containerPort}")
```

## Forward the Port: 

Use kubectl port-forward to access your application locally:
```
kubectl port-forward $POD_NAME 8081:80
```
![](./img/4a.port.forward.png)

## open your browser and go to:

```
http://127.0.0.1:8081
```
![](./img/4b.on.browser.png)

---


### Push to GitHub
```
git add .
git commit -m "update file"
git push origin master
```

## ✅ Conclusion

This project shows how Helm can simplify Kubernetes application deployments through templated configuration and version-controlled charts. Helm does not only helps maintain consistency across environments but also empowers teams to deploy and manage applications at scale with confidence.


