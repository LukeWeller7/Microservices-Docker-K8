1. Download the node image from DockerHub
```
docker pull node
```
2. Set up folder for the app-image 
```
mkdir app-image
```
3. Move into the folder the app folder and Dockerfile
4. Fill out the Dockerfile
```
FROM node:12
COPY app .
RUN npm install pm2 -g
RUN pm2 start app.js
EXPOSE 3000
CMD [ "node", "app.js" ]
```
5. Build the docker image
```
docker build -t lpweller/test-app:v4 .
```
6. Run the app on port 3000
```
docker run -d -p 3000:3000 lpweller/test-app:v4
```


### Connecting the DB to the App
These contain the step on how to connect the app and db, this is currently not working and I am figuring out the blocker at the moment.

1. Build a new image with the export command and re-run the app
```
FROM lpweller/test-app:v4
RUN export DB_HOST=mongodb://localhost:27017/posts
RUN node seeds/seed.js
RUN npm install
RUN pm2 kill
RUN npm install pm2 -g
RUN pm2 start app.js
EXPOSE 3000
CMD [ "node", "app.js" ]
```
2. Push build to DockerHub