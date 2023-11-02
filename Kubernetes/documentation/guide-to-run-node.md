# Guide to run node on K8
This is a step-by-step guide on how I deployed node to the localhost webpage using K8.

1. Create a file called node-deploy.yml
```
nano node-deploy.yml
```
2. Set up the yml file with details on how to build a deployment
```
apiVersion: apps/v1 # which api to use for deployment
kind: Deployment # What resource to create

metadata:
  name: node-deployment # name of deployment
spec:
  selector:
    matchLabels:
      app: node # look for this label to match with k8 service
  replicas: 3 # Number of pods to create
  template:
    metadata:
      labels:
        app: node # This label connects to any other k8 components
    spec:
      containers:
      - name: node # Name of pods
        image: lpweller/test-app:v4 # Use the image that runs node
        ports:
        - containerPort: 3000 # What port to run the container on
```
3. Create the deployment
```
kubectl create -f node-deploy.yml
```
4. Check the pods are running
```
kubectl get pods
```
```
Expected outcome:
---------------------------
NAME                               READY   STATUS    RESTARTS   AGE
nginx-deployment-588868d7f-6tqnr   1/1     Running   0          4h9m
nginx-deployment-588868d7f-8bbkm   1/1     Running   0          4h10m
nginx-deployment-588868d7f-v8h9s   1/1     Running   0          4h10m
node-deployment-64d7dd5ddc-ckctj   1/1     Running   0          20m
node-deployment-64d7dd5ddc-gs4dz   1/1     Running   0          20m
node-deployment-64d7dd5ddc-jg456   1/1     Running   0          20m
```
5. Create a file called node-service.yml
```
nano node-service.yml
```
6. Configure the yml file to create a service for the pods
```
apiVersion: v1
kind: Service

metadata:
  name: node-service
spec:
  selector:
    app: node
  ports:
    - protocol: TCP
      port: 3000
      targetPort: 3000
  type: LoadBalancer # Also can use NodePort
```
7. Apply the service to the K8 Cluster
```
kubectl apply -f node-service.yml
```
8. Check the service is running (Will also display the Cluster)
```
kubectl get service
```
```
Expected Output:
---------------------------
NAME            TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
kubernetes      ClusterIP   10.96.0.1       <none>        443/TCP          5h18m
nginx-service   NodePort    10.96.36.45     <none>        80:30352/TCP     63m
node-service    NodePort    10.97.162.248   <none>        3000:30992/TCP   14s
```
9. Go to the webrowser and access localhost:80 to find node page. (If used NodePort go to port specified in Port from service)