## Commands for Docker
```
docker run -d -p 80:80 nginx # Starts a new container 
# -d stands for detached mode
# -p which port
# 80:80 (port to use : port to replace)

docker ps # Shows the containers running
docker images # Shows a list of the images

docker stop <id> # Stops the specified container
docker start <id> # Starts the specified container

docker rmi 9c7a54a9a43c -f
rmi (remove image)
-f (force)

docker exec -it be5610a85479 sh
(docker execute interactive <container id> shell)
```
Find the Index file inside the container
```
apt install sudo
apt update -y
apt upgrade -y
cd /usr
cd share
cd nginx
cd html
cat index.html
apt install nano
sudo nano index.html
```