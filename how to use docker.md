### How to download docker 
* visit https://get.docker.com
* There should be some command like ```curl -fsSL https://get.docker.com -o get-docker.sh```
* and ```sh get-docker.sh```
* Google docker compose and docker machine install, and then go to the github to get the most recent version
* run ```docker version```,  ```docker-machine version```,  ```docker-compose version``` to verify installation

### What is image and what is container
* An image is the application we want to run 
* A container is an instance of that image running as a process(a run time instance of an image -- what the image becomes in memory when executed -- an imgge with state, a user process)
* Docker's default image registry is call docker hub
* ```ps aux``` show me all running process

## Run docker container commands basic
*  ```docker container run``` always starts new container, use ```docker container start``` to start an existing stopped one
* ```docker container run --publish 80:80 --detach --name webhost nginx```
* ```docker container run -d -p 3386:3386 --name db -e MYSQL_RANDOM_ROOT_PASSWORD=yes mysql``` => use ```--env``` to pass in ```MYSQL_RANDOM_ROOT_PASSWORD=yes``` and then use ```docker container logs db``` to find password 
* with mysql image , as you can read on docker hub have to specify one of 3 options. 1 specify a password 2 use no password 3 -e MYSQL_RANDOM_ROOT_PASSWORD=true
* ```docker container stats``` => show live performance data for all containers
* ```docker container inpect mysql``` => show metadata about the container , startup config, volumns, networking etc)
* ```docker container run -it --name proxy nginx bash``` => ```-t``` and ```--tty``` are pseudo-tty that simulates a real terminal, like SSH. ```-i``` and ```--interactive```means keeping session open to receive terminal input. ```bash``` is the command line default input when container spins up 
* ```docker container run -it --name ubuntu ubuntu```
* ```docker container start -ai ubuntu``` => start a existing one
* ```docker container exec -it mysql bash``` => execute a running docker container eg, detached container. This will start a new process from the running process of the container
* ```docker container logs webhost``` => logs in webhost
* ```docker container top webhost``` => list process in specific container 
* ```docker container ls -a``` => list all the containers 
* ```docker container rm 63f 690 0de``` => remove the container -f to force remove
* ```docker image ls``` list images locally
* ```docker container stop $(docker container ls -aq)```, ```docker container rm $(docker container ls -aq)``` => -q means numeric ID
* ```docker pull alpine``` , ```docker image ls``` pull a image from docker hub

### Docker network 
* pass

### Assignment:CLI App Testing
* Use different Linux distro containers to check curl cli tool version
* Use two different terminal windows to start bash in both centos:7 and ubuntu:14.04, using -it
* Learn the docker container --rm option so you can clean up
* Ensure is installed and on latest version for that distro
  * ubuntu: apt-get update && apt-get install curl
  * centos: yum update curl
 * Check curl --version   
 ```
docker container run --rm --it centos:7 bash
yum update curl
curl --version
docker container run --rm --it ubuntu:14.04 bash
curl --version
```

### Assignment: DNS Round Robin Test
* Ever since Docker Engine 1.11, we can have multiple containers on a created network respond to the same DNS address
* Create a new virtual network (default bridge driver)
* Create two containers from elasticsearch:2 image
* Research and use --network-alias search when creating them to give then an additional DNS name to respond to 
* Run alpine nslookup search with --net to see the two containers list for the same DNS name
* Run centos curl -s search:9200 with --net multiple times until you see both "name" fields show

```
docker network create dude
docker container run -d --net dude --net-alias search elasticsearch:2
docker container run -d --net dude --net-alias search elasticsearch:2   // two container with the same alias
docker container ls 
docker container run --rm --net dude alpine nslookup search
docker container run --rm --net dude centos curl -s search:9200
docker container run --rm --net dude centos curl -s search:9200
docker container run --rm --net dude centos curl -s search:9200
```
