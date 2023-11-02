# Commands on Kubernetes
Here is a documentation that covers all the commands I have used so far with K8. Also included a short explaination for what each command does.
```
kubectl
```
This is the base command for K8, use this to check that K8 is running

```
kubectl get service
kubectl get svc
```
Both of these commands work the same. They provide a list of the services running in the Kubernetes Cluster
```
kubectl get deploy
```
This command shows all the deployments in the K8 Cluster
```
kubectl create -f <file name>
```
This command will create a resource dependent on the file name. (used for deployment and pods)
```
kubectl get pods
```
This command shows a list of the pods running
```
kubectl delete <resource type> <name>
```
This command will delete the resource that is specified (e.g. pod, service)
```
kubectl apply -f <file name>
```
This command will apply the resource specified in the file name (used for service)
