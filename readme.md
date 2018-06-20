# Kubernetes Tutorial

## Overview

**Deployments** are the high-level constructs that define an application (Typically described in a YAML file).

**Pods** are instances of a container in a deployment.

**Services** are endpoints that expose ports to the outside world.

## Install 
You need to install kubectl and minikube. You will also need virtualbox installed.

### Installing kubectl

###### MacOS
```
$ brew install kubectl
```
###### Debian (Ubuntu)
```
$ sudo apt-get update && sudo apt-get install -y apt-transport-https
$ curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
$ sudo touch /etc/apt/sources.list.d/kubernetes.list 
$ echo "deb http://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee -a /etc/apt/sources.list.d/kubernetes.list
$ sudo apt-get update
$ sudo apt-get install -y kubectl
```
###### As part of the Google Cloud SDK
```
$ gcloud components install kubectl
```

### Installing minikube
###### MacOS
```
$ curl -Lo minikube https://storage.googleapis.com/minikube/releases/v0.28.0/minikube-darwin-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/
```
###### Debian (Ubuntu)
```
$ curl -Lo minikube https://storage.googleapis.com/minikube/releases/v0.28.0/minikube-linux-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/
```

## Running kubernetes

### Starting and stoping minikube
You will need to start minikube before you can start playing around with kubernetes and kubectl commands.

```
$ minikube start
```

When you are done, you have blow everything away by stoping minikube again.

```
$ minikube stop
```

### Verify kubectl is working
```
$ kubectl cluster-info
```

### Run sample application

Here we will run up a single instance of a docker image, exposing port 8080 to the outside world.
```
$ minikube start
$ kubectl run hello-minikube --image=k8s.gcr.io/echoserver:1.10 --port=8080
$ kubectl expose deployment hello-minikube --type=NodePort
$ kubectl get pod
$ curl $(minikube service hello-minikube --url)
$ kubectl delete services hello-minikube
$ kubectl delete deployment hello-minikube
$ minikube stop   
```

### Using YAML files
See the nginx-deployment.yaml file that is part of this tutorial. This will deploy an nginx container.
```
$ kubectl apply -f ./nginx-deployment.yaml
$ kubectl expose deployment nginx-deployment --type=NodePort # same as running kubectl create -f nginx-service.yaml
$ minikube service nginx-deployment --url
$ curl <URL_FROM_PREVIOUS_COMAND>
$ kubectl describe pod nginx-deployment-56b8664f97-bj45s
```

### Other commands

To view the minikube kubernetes dashboard in the browser type the followig command:

```
$ minikube dashboard
```
or 

```
$ kubectl proxy 
```
and navigate to http://localhost:8001/ui

### Scale an existing deployment
```
$ kubectl scale --replicas=4 deployment/nginx-deployment
```
Update the service for the 4 replicas:
```
$ kubectl expose deployment nginx-deployment --type=LoadBalancer --port=8080 --target-port=8080 --name nginx-load-balancer
$ kubectl describe services nginx-load-balancer
```

