---
layout: post
title:  "Brief Docker introduction!"
---


## Brief Docker introduction 

Docker provides very simple and efficient way to pack your specific application. As stated here: 
```
Developing apps today requires so much more than writing code. Multiple languages, frameworks, architectures, and discontinuous interfaces between tools for each lifecycle stage creates enormous complexity. 
Docker simplifies and accelerates your workflow, while giving developers the freedom to innovate with their choice of tools, application stacks, and deployment environments for each project.
```

It makes easier to build different environments and swap them easily without corrupting your local operating system. 

We're basically creating light-weight isolated environments (images/containers) which have everything that you put into it.

### How to install Docker? 

Instructions for installing Docker can be found [here](https://docs.docker.com/engine/install/ubuntu/)

### Docker terms

#### What's Dockerfile? 

A Dockerfile is a simple text file that contains a list of commands that the Docker client calls while creating an image. It's a simple way to automate the image creation process. The best part is that the commands you write in a Dockerfile are almost identical to their equivalent Linux commands. This means you don't really have to learn new syntax to create your own dockerfiles.

#### What's docker image? 

A Docker image is a read-only template that contains a set of instructions for creating a container that can run on the Docker platform.

#### What's docker container? 

A container is a standard unit of software that packages up code and all its dependencies so the application runs quickly and reliably from one computing environment to another. A Docker container image is a lightweight, standalone, executable package of software that includes everything needed to run an application: code, runtime, system tools, system libraries and settings.


#### Dockerfile commands: 

| Command    | Purpose                                                                                                                                       |
|------------|-----------------------------------------------------------------------------------------------------------------------------------------------|
| FROM       | To specify the parent image.                                                                                                                  |
| WORKDIR    | To set the working directory for any commands that follow in the Dockerfile.                                                                  |
| RUN        | To install any applications and packages required for your container.                                                                         |
| COPY       | To copy over files or directories from a specific location.                                                                                   |
| ADD        | As COPY, but also able to handle remote URLs and unpack compressed files.                                                                     |
| ENTRYPOINT | Command that will always be executed when the container starts. If not specified, the default is /bin/sh -c                                   |
| CMD        | Arguments passed to the  entrypoint. If ENTRYPOINT is not set (defaults to /bin/sh -c), the CMD  will be the commands the container executes. |
| EXPOSE     | To define which port through which to access your container application.                                                                      |
| LABEL      | To add metadata to the image.    

#### Docker cheat sheet: 

##### How to build Dockerfile? 

Enter corresponding directory and run following command to build docker image: 
```
docker build -t <image_name>:<tag_name> <dockerfile_path> 
```

For concrete example (ros-kinetic with gazebo) run following: 
```
docker build -t ros_gazebo_img:melodic_11
```

##### How to create container out of the Dockerfile? 

In order to create container from Dockerfile run following commmand:
```
docker run -it --network host --privileged -v /dev:/dev --name <container_name> <img_name>:<tag_name> 
```

Run docker container with GUI support 
```
docker run -it --network host --gpus all --privileged -e DISPLAY=$DISPLAY -v /dev:/dev -v /tmp/.X11-unix:/tmp/.X11-unix \
       --name <container_name> <img_name>:<tag_name> 
```
Also, before using param `--gpus` make sure you've installed `nvidia-container-toolkit` as follows: 

```
sudo apt-get update
sudo apt-get install -y nvidia-container-toolkit
```
Make sure you have latest drivers for your GPU. 

##### How to start existing docker container after stopping it? 
```
docker start -i <container_name> 
```

##### How to open running container in new bash? 
```
docker exec -it <container_name> bash 
```

##### How to kill, stop, remove container? 

Kill container: 
```
docker kill  <container_name> 
```

Stop container: 
```
docker  stop <container_name> 
```

Remove container: 
```
docker rm <container_name> 
```

Attach existing detached cotainer: 
```
docker attach <container_name> 
```

Clean dangling images and stopped containers (bear in mind that this deletes 
every container that's not currently running, not recommended): 
```
docker system prune 
```

#### [Docker-CLI reference](https://docs.docker.com/engine/reference/commandline/build/)

#### How to create configuration file that runs multiple docker files at once? 

In order to run multiple docker containers which are build from docker images in certain configuration 
use [docker-compose](https://docs.docker.com/compose/) 


