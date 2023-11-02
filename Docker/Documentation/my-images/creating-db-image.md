# How I built my Database Image on DockerHub
1. Download the mongo image from DockerHub
```
docker pull mongo
```
2. Set up folder for the db-image 
```
mkdir db-image
```
3. Create a file called mongod.conf and set BindIp (If unsure what the file to replace is, exec into mongo to check.)
```
# mongod.conf

# for documentation of all options, see:
#   http://docs.mongodb.org/manual/reference/configuration-options/

# Where and how to store data.
storage:
  dbPath: /var/lib/mongodb
#  engine:
#  wiredTiger:

# where to write logging data.
systemLog:
  destination: file
  logAppend: true
  path: /var/log/mongodb/mongod.log

# network interfaces
net:
  port: 27017
  bindIp: 0.0.0.0


# how the process runs
processManagement:
  timeZoneInfo: /usr/share/zoneinfo

#security:

#operationProfiling:

#replication:

#sharding:

## Enterprise-Only Options:

#auditLog:
```
4. Fill out the Dockerfile
```
FROM mongo
COPY mongod.conf /etc/mongod.conf.orig
EXPOSE 27017
CMD [ "mongod" ]
```
5. Build the docker image
```
docker build -t lpweller/test-db:vv .
```
6. Run the app on port 27017
```
docker run -d -p 27017:27017 lpweller/test-db:v4
```


New Dockerfile
```
FROM mongo
COPY mongod.conf /etc/mongod.conf
EXPOSE 27017
CMD [ "mongod", "--config", "/etc/mongod.conf" ]
```