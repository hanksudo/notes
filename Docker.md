# Docker

Docker Note

## Installation

```bash
brew cask install docker-toolbox
```

## Info

- [Docker Store](https://hub.docker.com/)
- [hanksudo dockerfiles](https://github.com/hanksudo/dockerfiles)
- [Docker hub mirror switch](https://github.com/hanksudo/docker-mirror-switch)
- [Get Docker CE for Ubuntu | Docker Documentation](https://docs.docker.com/install/linux/docker-ce/ubuntu/)

## Showcase

- [Get Started, Part 1: Orientation and setup | Docker Documentation](https://docs.docker.com/get-started/)


### Hello World

```bash
# docker will auto pull image, if image not exists
docker run hello-world
```

### Nginx Web Server

```bash
docker run -d -p 8000:80 nginx
curl http://localhost:8000
```

## Docker basic command usage

``` bash
docker version
docker info

# Search image
docker search nginx

# Docker will auto pull image if not exists
docker run alpine echo 'Hello Docker!'

# remove container after run it.
docker run --rm alpine echo 'Hello Docker!'

# interactve container
docker run -it alpine bash

# command
docker run alpine /bin/sh -c 'cat /etc/*-release'

# Start a daemonized Hello world
docker run -d ubuntu /bin/sh -c "while true; do echo hello world; sleep 1; done"

# Listing containers
docker ps     # show running container
docker ps -a  # show all container (include not running)
docker ps -l  # show latest created container

# Fetch the logs of a container
docker logs thirsty_wilson
docker logs -f thirsty_wilson  # follow output
docker logs -t thirsty_wilson  # with datetime

# Stop container
docker stop thirsty_wilson

# restart container
docker restart thirsty_wilson

# Run web application on background and publish all exposed ports to random ports
docker run -d -P training/webapp python app.py

# Run web application on background and exposed port 5000
docker run -d -p 5000:5000 training/webapp python app.py

# assign name to container
docker run -d --name=thirsty_wilson -p 5000:5000 training/webapp python app.py

# List port mappings for the CONTAINER
docker port thirsty_wilson

# Run a command in a running container
docker exec thirsty_wilson ls

# bash / ssh into a running container
docker exec -it thirsty_wilson bash

# Display the running processes of a container
docker top thirsty_wilson

# Copy file from container to host
docker cp thirsty_wilson:/etc/nginx/nginx.conf .

# Return low-level information on a container or image
docker inspect focused_mcclintock
docker inspect -f '{{ .NetworkSettings.IPAddress }}' focused_mcclintock

# Remove container
docker stop thirsty_wilson
docker rm thirsty_wilson
docker rm -f thirsty_wilson
```

## Images

``` bash
docker search nginx
docker search hanksudo

# Download public image from Docker Hub
docker pull hanksudo/ubuntu-12.04.5:latest

# List images
docker images

docker run -it --rm hanksudo/ubuntu-12.04.5 /bin/bash

# modift and commit (f510e8084141 is container id)
docker commit -m="test commit" -a="Hank Wang" f510e8084141 hanksudo/ubuntu-12.04.5:v2
```

## Build own Image

- [Get Started, Part 2: Containers | Docker Documentation](https://docs.docker.com/get-started/part2/)
- [Best practices for writing Dockerfiles](https://docs.docker.com/engine/userguide/eng-image/dockerfile_best-practices/)

Create an empty directory. Change directories (`cd`) into the new directory, create a file called `Dockerfile`

```bash
mkdir -p docker-helloworld
touch Dockerfile
```

**Dockerfile**

```docker
# Hello Docker
FROM alpine
CMD ["/bin/echo", "Hello、世界！"]
```

### build and push to Docker hub

```bash
# build image
docker build -t docker-helloworld .
docker images

# Run a new container
docker run docker-helloworld
docker run --rm docker-helloworld  # remove container after run

# Add Tag to image
docker tag docker-helloworld "hanksudo/docker-helloworld:v1"

# Login to Docker hub
docker login

# Push
docker push "hanksudo/docker-helloworld"

# remove local image
docker rmi "hanksudo/docker-helloworld"
```

## Link Containers

```bash
docker run -d --name db training/postgres
docker run -d -P --name web --link db:db training/webapp python app.py
```

## Shortcuts

- [Docker shortcuts](https://github.com/hanksudo/dotfiles/blob/master/zsh/zshrc#L135)

1. `dcleanimages` - Remove all unused images
2. `dcleancontainers` - Remove all exited containers.

## Connect from containers to host

`docker.for.mac.host.internal`

## Install docker and docker-compose on Ubuntu 16.04

```bash
curl -fsSL get.docker.com -o get-docker.sh
sudo sh get-docker.sh

sudo curl -L "https://github.com/docker/compose/releases/download/1.23.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose

sudo usermod -aG docker ubuntu
sudo setfacl -m user:ubuntu:rw /var/run/docker.sock
```

## Lazydocker

https://github.com/jesseduffield/lazydocker

```bash
curl https://raw.githubusercontent.com/jesseduffield/lazydocker/master/scripts/install_update_linux.sh | bash
```
