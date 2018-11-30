# Kubernetes Commands

## Deploy nginx image
1. kubectl version
1. kubectl cluster-info
1. kubectl run --image nginx webserver
1. kubectl get deploy
1. kubectl get rs
1. kubectl get pod
1. kubectl get
1. kubectl describe deploy webserver
1. kubectl port-forward webserver-5748cdbbfc-f5v58 5000:80
1. kubectl expose deploy webserver --port 80 --type NodePort
1. kubectl get services <-- to get random port

### Troubleshoot
If you see error: 'Unable to connect to the server: dial tcp 192.168.99.100:8443: i/o timeout'

`$ minikube start`

## Build your own image
`$ docker build -t kubernetes-node .` <-- the . is the path to the docker file

## Deploy your own image
google kubernetes deployment yaml for template and update it to match above image,port,replicas etc


    `imagePullPolicy: IfNotPresent`


`$ kubectl apply -f deployment.yaml` -f = file

`$ kubectl expose deploy kubernetes-node-deployment --type NodePort`

`$ kubectl get service kubernetes-node-deployment -o=yaml` <-- could save this and commit it as part of project

## Update your image
1. `$ docker build -t kubernetes-node:v2 .` 
1. Update version in deploymemnt.yaml
1. `$ kubectl apply -f deployment.yaml` 