# AdvancedDocker



#### 1. Task 1
**File IO**: You want to create a container for a legacy application. You succeed, but you need access to a file that the legacy app creates.

* Create a container that runs a command that outputs to a file.
* Use socat to map file access to read file container and expose over port 9001 (hint can use SYSTEM + cat).
* Use a linked container that access that file over network. The linked container can just use a command such as curl to access data from other container.

Container1 contains the code to create a new file and output to it:
```
FROM ubuntu:14.04
MAINTAINER Shraddha Naik

RUN apt-get update  
RUN apt-get install -y socat
RUN touch temp.txt 
RUN echo "Hello this is Shraddha Naik" > temp.txt

CMD socat TCP-LISTEN:9001 SYSTEM:'cat temp.txt'

```
To run it we use:
```
cd FileIO/container1
sudo docker build -t file1 .
sudo docker run -d --name container1 file1

```
Container2 is a linked container which accesses the data from the other container using curl. Its dockerfile contains the following:
```
FROM ubuntu:14.04
MAINTAINER Shraddha Naik

RUN apt-get -y install curl
```
To run it we use the following command:
```
cd FileIO/container2
sudo docker build -t file2 .
sudo docker run --link container1:server --rm -it --name container2 file2 curl server:9001
```
#### 2. Task 2 
**Ambassador pattern**: Implement the remote ambassador pattern to encapsulate access to a redis container by a container on a different host.

* Use Docker Compose to configure containers.
* Use two different VMs to isolate the docker hosts. VMs can be from vagrant, DO, etc.
* The client should just be performing a simple "set/get" request.
* In total, there should be 4 containers.

I create two ec2 instances, each having docker-compose.yml files.
In the first instance we create two containers : ambassador and redis
In the second instance we create two containers : ambassador and redis_client

On the server :
```
docker-compose up -d
```
On the client :
```
docker-compose run redis_client
```

#### 3. Task 3 
**Docker Deploy**: Extend the deployment workshop to run a docker deployment process.

* A commit will build a new docker image.
* Push to local registery.
* Deploy the dockerized [simple node.js App](https://github.com/CSC-DevOps/App) to blue or green slice.
* Add appropriate hook commands to pull from registery, stop, and restart containers.

I followed the steps in the Deployment workshop and created the directory structure for deploy.
Create a post-commit hook for the application.
Created a post-receive hook for blue and green slice.

To check the deployment I make a small change in main.js, then for blue :
```
git add main.js
git commit -m "test"
git push blue master
```
for green :
```
git add main.js
git commit -m "test"
git push green master
```


