# Docker Cheat Sheet

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

### Part 2










*** @author: Enes Kilicaslan ***

*** @ref: Code School ***
