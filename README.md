# Docker

Docker is a tool designed to make it easier to create, deploy and run application by using container. Docker is a open-source software. It's written in Go.
Docker, Inc. is a company that developed docker tool.


What is container?

Containers allow a developer to package up an application with all of the parts it needs, such as libraries and other dependencies, and deploy it as one package. By doing so, thanks to the container, the developer can rest assured that the application will run on any other Linux machine regardless of any customized settings that machine might have that could differ from the machine used for writing and testing the code.

Backgroud (bit of a history):

Actually the usage of container is not something which came into existance with docker. The container existed ever since the origin of Linux OS. But back then there was not formal way of containerising the application which is now very well marketed and fullfilled by Docker Inc company.

In Linux OS before docker, companies like Google used to tweek Linux OS kernel technologies like namespaces and cgroups (Control Group) to deploy their application. 
Docker company which was initially dotCloud, Inc. came up with this idea of leveraging the same underlying technologies like namespaces and cgroup together with Union file system and made it product known as Docker.


### Namespaces
These namespaces provide a layer of isolation. Each aspect of a container runs in a separate namespace and its access is limited to that namespace.

### Control Groups
A cgroup limits an application to a specific set of resources. Control groups allow Docker Engine to share available hardware resources to containers and optionally enforce limits and constraints. For example, you can limit the memory available to a specific container.

### Union File System
Union file systems, or UnionFS, are file systems that operate by creating layers, making them very lightweight and fast. Docker Engine uses UnionFS to provide the building blocks for containers. Docker Engine can use multiple UnionFS variants, including AUFS, btrfs, vfs, and DeviceMapper.


## Docker Architecture

Docker architecture can be found [here](https://docs.docker.com/get-started/overview/#docker-architecture)


## Docker Installation

Docker is available for Mac, Windows and Linux as Docker for Mac and Docker for Windows and Docker for Linux.

For Windows 10 Home edition as Hyper-V is not available we can't use Docker for Windows, in which case there is option to use Docker toolbox.

Install Docker toolbox [here](https://docs.bitnami.com/containers/how-to/install-docker-in-windows/)

Install Docker for Windows [here](https://docs.docker.com/docker-for-windows/install/)

The above installation are for development/test usage and not for production.



### What are images?
An image is a stopped container. It's not a full blob object instead consists of the layers that's what makes it lightweight. 
An image consists of a base image like alpine (linux os) or ngnix(reverse proxy/web server) and then bunch of layers on top of it as per the usecase.

### Containerizing an App

Dockerfile is used to build docker image with application code and all the environment it needs.
Dockerfile contains the set instruction to build image.
An image is nothing but a bunch of layers, it's not a single blob object with all files. That's why it's lightweight.


#### Working with Dockerfile
For working with Dockerfile in windows using docker toolbox and running virtual box 

1. Created a Dockerfile [here](https://github.com/goutamsh/docker-learning/blob/master/nginx/Dockerfile)

2. placed the Dockerfile in the same location as the code to be copied to conatiner

3. Ran below command to build image out of above Dockerfile
```
docker build -t nginx-hello-world:1.0 .  

docker build -t alpine-node-hello-world:1.0 .
```

4. To run container from the above image we created 
```
docker run --name nginxHW -d -p 8080:80 nginx-hello-world:1.0

docker run --name alpineNodeHW -d -p 8088:8080 alpine-node-hello-world:1.0
```

5. To know the IP address of the docker tool box, run below command 
```
docker-machine ip default
```

What is Docker Machine?

Machine lets you create Docker hosts on your computer, on cloud providers, and inside your own data center. It creates servers, installs Docker on them, then configures the Docker client to talk to them.

Docker Host is a server machine where the docker daemon runs. Docker host can be bare metal, VM machine, iso, image hosted on some cloud etc.

6. use above IP and port 8080 to see the webpage like http://192.168.99.102:8080
and 
http://192.168.99.102:8088/


#### Pushing new image to docker hub

1. Login to docker Hub
```
docker login --username=goutamsh
```
After Login Success

2. Check the list of current images and see which one need to be pushed
```
docker images
```

3. Create a tag for the image which needs to be pushed
```
docker tag 0afb4ed988dd goutamsh/nginx-hello-world:1.0

docker tag c9dc785280ca goutamsh/alpine-node-hello-world:1.0
```

4. Push the image for example 
```
docker push goutamsh/nginx-hello-world:1.0

docker push goutamsh/alpine-node-hello-world:1.0
```
5. The pushed image can be found [here](https://hub.docker.com/repository/docker/goutamsh/nginx-hello-world)


#### Docker multistage build

https://github.com/dockersamples/atsea-sample-shop-app/tree/master/app

### Docker swarm mode

Docker swarm mode is a cluster of docker nodes. It consists of master node and worker node.
It's secure as it's used PKI encryption with certificates for communication between the mangers and worker nodes.

docker swarm init --advertise-addr 192.168.99.102

docker node ls

To add a worker to this swarm, run the following command:

docker swarm join --token SWMTKN-1-0z0hyja0vv6x81kt44r7yyl5et6bv5js1h5cxxnwbv6quhlqy4-13r6erfx4abp0ypsywvgm4p8y 192.168.99.102:2377

To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.

docker swarm join-token manager

docker swarm join-token worker


### Services

docker service create -d --name my-first-svc --replicas 3 -p 8080:8080 goutamsh/alpine-node-hello-world:1.0

docker node ps

docker service ls


### Secrets 


https://docs.docker.com/engine/reference/commandline/secret_create/


docker secret create my_secret secretfile.txt

docker secret ls


create service with secret

docker service create -d --name my-first-svc --secret my_secret --replicas 3 -p 8080:8080 goutamsh/alpine-node-hello-world:1.0        

login to container to see if secret is accessible

docker container exec -it cc sh  

ls /run/secrets/
Inside contaner we can see secret un-encrypted


### Volumes for persistence

https://docs.docker.com/storage/volumes/

docker volume create my-vol

docker volume ls

docker volume inspect my-vol

mounting the volume to service 

docker service create -d --name my-first-svc --secret my_secret --mount source=my-vol,target=/app --replicas 3 -p 8080:8080 goutamsh/alpine-node-hello-world:1.0    

docker exec -it e9 sh

ls /app

### Stacks 


docker stack deploy --compose-file stackfile_v1.yml my-first-stack

docker stack ls


### Container Lifecycle


Container can be started, stopped, restarted and the deleted.
Starting a stopped container will basically resume. All data is retained at the time of stopping. All the data sticks around until we tell container to be deleted.


### Few Docker Commands
```
docker info

docker ps

docker images


docker image inspect nginx-hello-world:1.0

docker stop nginxHW

docker container rm nginxHW

docker image rm goutamsh/nginx-hello-world:1.0


docker login --username=goutamsh

docker run --name nginxHW -d -p 8080:80 goutamsh/nginx-hello-world:1.0

docker logs alpineNodeHW

docker history alpine-node-hello-world:1.0


--Running commands inside container

docker exec <container_name> <command_to_exc>
docker exec -it <container_name> sh
--<container_name> can be few characters of the container_id from "docker ps" command


```
### Configuring local docker client to connect to remote docker host

Reference [here](https://www.kevinkuszyk.com/2016/11/28/connect-your-docker-client-to-a-remote-docker-host/)


### Springboot with docker

https://spring.io/guides/gs/spring-boot-docker/

Springboot application is [here](https://github.com/goutamsh/docker-learning/tree/master/springboot_with_docker/springboot-docker)

It exposes an endpoint on port 9100 (so application can be accessed from http://localhost:9100 if this springboot application is run directly on local machine or from docker-machine ip)

Dockerfile to containerize the above springboot is [here](https://github.com/goutamsh/docker-learning/blob/master/springboot_with_docker/Dockerfile)

Commands to build

```
docker build -t springboot-with-docker .

docker run -p 9200:9100 springboot-with-docker

```

The above command is mapping external port 9200 to internal port 9100 on the container.
so application can be accessed with http://<docker-machine ip>:9200

# Reference:

1. https://opensource.com/resources/what-docker

2. https://docs.docker.com/get-started/overview/#docker-architecture

