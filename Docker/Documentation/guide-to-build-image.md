# How to build an image on Docker 
This is a guide on how to build an image on Docker, this is an example build that I completed to build a nginx.
### Step by Step Guide:
1. Set up file and folder structure
```
mkdir nginx-image && cd nginx-image
mkdir files
touch .dockerignore
```
2. Create an index.html file and fill out the page
```
cd files
nano index.html
```
```html
<html>
  <head>
    <title>Dockerfile</title>
  </head>
  <body>
    <div class="container">
      <h1>My App</h1>
      <h2>This is my first app</h2>
      <p>Hello everyone, This is running via Docker container</p>
    </div>
  </body>
</html>
```
3. Create a default file in files folder
```
nano default
```

```
server {
    listen 80 default_server;
    listen [::]:80 default_server;
    
    root /usr/share/nginx/html;
    index index.html index.htm;

    server_name _;
    location / {
        try_files $uri $uri/ =404;
    }
}
```
4. In nginx-image folder create a Dockerfile and add content
```
cd ..
nano Dockerfile
```
```
FROM ubuntu:18.04  
LABEL maintainer="personalemail@dockerhub.com" 
RUN  apt-get -y update && apt-get -y install nginx
COPY files/default /etc/nginx/sites-available/default #local path to default / instance path to default
COPY files/index.html /usr/share/nginx/html/index.html #local path to index.html / instance path to index.html
EXPOSE 80 #port
CMD ["/usr/sbin/nginx", "-g", "daemon off;"]
```
```
# Alternative Dockerfile option 
# from which image
FROM nginx

# what to copy into the container from localhost - index.html
COPY files/index.html /usr/share/nginx/html/

# port 80
EXPOSE 80

# CMD specific instructions
CMD ["nginx", "-g", "daemon off;"]
```
5. Build the docker image (same folder as Dockerfile file)
```
docker build -t nginx_build:v1
```
6. Optional - test the image
```
docker images
docker run -d -p 81:80 nginx_build:v1
docker ps
```
7. Log into docker
```
docker login
```
8. Tag image with docker username
```
docker tag nginx_build:v1 lpweller/nginx_build:v1
```
9. Push to dockerhub
```
docker push lpweller/nginx_build:v1
```


Blocker: 
- When attempting to run a container, it closes automatically. (Fixed)
  - Add `sleep infinity` to the end of run command