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



```
### Configuring local docker client to connect to remote docker host

Reference [here](https://www.kevinkuszyk.com/2016/11/28/connect-your-docker-client-to-a-remote-docker-host/)



# Reference:

1. https://opensource.com/resources/what-docker

2. https://docs.docker.com/get-started/overview/#docker-architecture

