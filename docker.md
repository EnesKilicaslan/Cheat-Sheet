# Docker Cheat Sheet

## Docker Containers

### Running a container

To start running a container, type docker container run followed by the name of the image used to make the container, then a colon : followed by the version number of the image used to make the container.

```sh

  docker container run httpd:2.4

```

### Mapping ports

The -p means "publish ports". The first number is the port on the host we want to open up, and the second number is the port in the container we want to map it to.
Port 80 is a standard port for handling web requests, but we could map any port on the host to the container. Let's try mapping 9999 on the host to 80 on the container.

```sh

  docker container run -p 9999:80 httpd:2.4

```

### Accessing a container
The docker container exec command lets us run **docker container exec** commands against a running container.

Each container gets a unique ID, and Docker also assigns them a random name. You can use either the container ID or the container name whenever you're running a command that needs you to specify a container


```sh

  docker container exec [container_name] du -mh

```


### Attaching a shell to a container
Running commands is cool, but we can also attach a shell to those containers and browse them just like they were a full operating system  — which they kind of are.
The **-i** and **-t** flags can be passed to the **exec** command to keep an interactive shell open, and you can specify the shell you want to attach after the container ID

```sh

docker container exec -it [container_name] /bin/bash

```

### Update the ENV in a Container
It’s kind of annoying to have to type **/usr/games/** before the fortune command, so let’s update the **PATH** environment variable in the container so that we don’t have to type all of that anymore.

```sh

  PATH=$PATH:/usr/games/
  export PATH
  fortune
```

##DockerFiles

### Docker File

A docker file is a special formatted text file where you can add a list of instructions that will result in a new image that can be used to make a new container.

The filename is important — it's always Dockerfile with a capital D.

The first line of a Dockerfile is usually the **FROM** keyword followed a base image name. Every other instruction in the Dockerfile following the FROM instruction will create a new image that blends together everything in the base plus the modifications we're making in the rest of the Dockerfile

we can use the Dockerfile to expose a port inside of its associated container, by **EXPOSE** command

 Dockerfile we can run any commands as the image is being built with the **RUN** command

Since we can distribute Dockerfiles to other developers, it's a good idea to put our contact information in them with the **LABEL** instruction.

```docker

FROM httpd:2.4
EXPOSE 80
RUN apt-get update && apt-get install -y fortunes
LABEL maintainer="moby-dock@example.com"

```


### Building image from a DockerFile

go. We can use that Dockerfile to build a new image with the **docker image build** command. The **--tag** command is a useful option to pass to docker build that lets us specify the name of our new image followed by a version number

End the command with a single '.'(dot) so it knows to look for the Dockerfile in the same folder that the command is run in.

**docker image ls** command is used to see all the docker images


## Docker Volumes

if the image you're building a container with does not already contain applications files, you will need an extra step to get them into your container

  1. copy a file into a container from the command line.
  2. copy a file into a image with instructions in a DockerFile.

Containers don't persist data, solution is data volumes.
Data volumes expose files on your host machine to the container.

### copying files into a running container

Like many Unix commands, the order of the arguments after the cp are important. First, we write a path to the location of the file we want to copy on your host machine, and after that we write the container ID, followed by a colon, followed by a path in the container where we want the file to be copied to.


1. One way to get a file into a container is with the **docker container cp** command.

  ```docker

  docker container cp file_name container_name:/path/

  ```

2. Another way to copy a file into a container is to write a COPY instruction in a Dockerfile

  ```docker

    #Inside of DockerFile
    COPY file_name path

  ```



  The advantage of this approach over the docker container cp command is that each new container we create with the custom image will have the file in it versus having to run the command-line command each time we make a new container.



### Example:

The following commands run a docker from image web-server:1.1 mapping  port 80 on the host to port 80 on the container and also use the **--detach** flag to make the container run in the background.

```docker

  docker container run -p 80:80 --detach web-server:1.1


```


### Creating a Volume

The first two approaches involved copying a file into a container. But as soon as the container is modified and stopped, all of those changes disappear. This is a problem if we’re using a container for local development, and one way to fix this problem is to use a data volume to create a connection between files on our local computer (host) and the filesystem in the container. **-v** flag is used to create a link between a folder on host machine and folder on the container

```docker

  docker run -d -p 80:80 -v /host_folder:container_folder web-server:1.1

```



*********************

***useful links:***

- [Docker Documentation Site](http://https://docs.docker.com)
- [Docker Store to find images](https://store.docker.com)

*********************



***@author: [Enes Kilicaslan](http://eneskilicaslan.github.io)***

***@ref: [Code School](https://www.codeschool.com/courses/try-docker)***
