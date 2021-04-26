# Docker 

## Install docker on ubuntu 
[This link](https://docs.docker.com/engine/install/ubuntu/) has the details installation of docker. 
Older versions of Docker were called docker, docker.io, or docker-engine. If these are installed, uninstall them:
```
sudo apt-get remove docker docker-engine docker.io containerd runc
```
There are few methods of installing docker. We will follow easy instllation method which is installaing docker using script file. 
```
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
```
Check Docker version 
```
docker --version
```

## Basic Docker Command
- `run` command is use to run a container from an image 
```
docker run nginx
```
-  `ps` command lists all runnning container
```
docker ps
```
- To see all container 
```
docker ps -a
```

-  `stop` is used to  stop a container 
```
docker stop <name>
to get the name of the container run ps command
```

- `rm` - Remove an exited container permanently

```
docker rm <name>
````

- `images` List images  and their sizes 

```
docker images
```

- `rmi` - To remove an unsed images
```
docker rmi <image_repo_name>
you must stop and delete all dependent containers to be able to delete an image  
```
- `pull` - To pull an image from docker repositoty
```
docker pull nginx
```
Note:  A container only lives as long as the process inside it. 

- `exec` is use to run a command inside a container
```
docker exec <container_id> cat /etc/relese
```
- To see all the details of container 
```
docker inspect <container_name/id>
```
It returns all the details in a json format. 



## Acces Docker Bash 
```
docker exec -it <mycontainer> bash
```
## PORT Mapping 
- expose docker container port to the outside
```
docker run -p 80:5000 kodeloud/simple-webapp
here 80 is the external ip and 5000 is the container internal ip 
```

## Folder Mapping 
all data inside docker container destroyed when docker container deleted. So to solve this problem, a folder outside docker container can be mapped to internal folder. 
```
docker run -v /out/dir:/in/dir mysql
```

## See to container logs
```
docker logs <container_id/name>
```
## Run container in detached or attached mode
- Running container in the background use `-d` flag. This is called detached mode.This will run the container in the background mode 
```
docker run -d ubuntu sleep 100
``` 
to run the container in the foreground mode(attahce) run the following command 
```
docker ps (get the container_id)
docker attach <container_id>
```

## Creating own image  
1. Create a docker file  and specify the configuration 
2. `docker build DockerFile -t user_name/app_name` 
3. To make available in public dockerhub registry `docker push user_name/app_name`

### Rules of creating DockerFile 
1. Docker file is a text file written in a specific format docker can understand. 
2. It consists of instructions and arguments
3. Everything in a instruction is left and caps letter 
4. Everything in the right is an arguments to those instructions. 
5. Every instruction in the file create a lyered architecture. first command create first layer and second instruction create second layer and so on. 

```
FROM Ubuntu

RUN apt-get update
RUN apt-get install python 

RUN pip install flask
RUN pip install flask-mysql

COPY . /opt/source-code

ENTRYPOINT  FLASK_APP = opt/source-code/app.py flask run 

```
- Every docker image is based on another docker image or OS. 
- first line  is mentioned the base image and all docker file must start with a `FROM` instruction.  
- `RUN` insrtruction instruct docker to run particular command on docker base image. 
- `COPY` instruction copy files from local folder to image. in the example source code in the current folder and copying it to `/opt/source-code
- `ENTRYPOINT` allows the image that will be run the image as a container.  


## Demo
sudo docker run -it ubuntu bash
apt-get update
apt-get install -y python
apt-get install pip

## Install Docker Compose
docker-compose needs to install separeltey using the following command 
```
sudo curl -L "https://github.com/docker/compose/releases/download/1.25.5/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

Aafter installation apply executable permissions to the binary:
```
sudo chmod +x /usr/local/bin/docker-compose
````

## Difference between DockerFile and docker compose yml file
So the workflow looks like this:

1. reate Dockerfiles to build images. 
2. Define complex stacks (comprising of individual containers) based on those Dockerfile images from within docker-compose.yml. 
3. Deploy the entire stack with the docker compose command.

And that is the fundamental difference between Dockerfile and docker-compse.yml files.

## Docker Compose command
- compose docker in detached mode
```
docker-compose up -d
```
Specify a image name while building. This will override folder name  
````
docker-compose --project-name <img_name> up

## Docker Build 
```
docker build . -t voting-app
```
here voting-app is the image tag name 

## Validate compose file without running 
docker-compose config


## List all container 
docker ps -a

#remove stopped container 
docker rm containerID



In case docker rmi <image-id> didn't work, try this:

Stop all running containers

docker stop $(docker ps -aq)
Remove all containers

docker rm $(docker ps -aq)
Remove all images

docker rmi $(docker images -q)