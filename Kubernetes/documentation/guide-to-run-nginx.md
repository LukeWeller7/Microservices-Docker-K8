# Guide to run nginx on K8
This is a step-by-step guide on how I deployed nginx to the localhost webpage using K8.

1. Create a file called nginx-deploy.yml
```
nano nginx-deploy.yml
```
2. Set up the yml file with details on how to build a deployment
```
apiVersion: apps/v1 # which api to use for deployment
kind: Deployment # What resource to create

metadata:
  name: nginx-deployment # name of deployment
spec:
  selector:
    matchLabels:
      app: nginx # look for this label to match with k8 service
  replicas: 3 # Number of pods to create
  template:
    metadata:
      labels:
        app: nginx # This label connects to any other k8 components
    spec:
      containers:
      - name: nginx # Name of pods
        image: lpweller/nginx-254:latest # Use the image that runs nginx
        ports:
        - containerPort: 80 # What port to run the container on
```
3. Create the deployment
```
kubectl create -f nginx-deploy.yml
```
4. Check the pods are running
```
kubectl get pods
```
```
Expected outcome:
---------------------------
NAME                               READY   STATUS    RESTARTS   AGE
nginx-deployment-588868d7f-6tqnr   1/1     Running   0          59m
nginx-deployment-588868d7f-8bbkm   1/1     Running   0          60m
nginx-deployment-588868d7f-v8h9s   1/1     Running   0          60m
```
5. Create a file called nginx-service.yml
```
nano nginx-service.yml
```
6. Configure the yml file to create a service for the pods
```
apiVersion: v1
kind: Service

metadata:
  name: nginx-service
spec:
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: LoadBalancer # Also can use NodePort
```
7. Apply the service to the K8 Cluster
```
kubectl apply -f nginx-service.yml
```
8. Check the service is running (Will also display the Cluster)
```
kubectl get service
```
```
Expected Output:
---------------------------
NAME            TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
kubernetes      ClusterIP      10.96.0.1       <none>        443/TCP        151m
nginx-service   LoadBalancer   10.101.224.32   localhost     80:32501/TCP   26m
```
9. Go to the webrowser and access localhost:80 to find nginx page. (If used NodePort go to port specified in Port from service)