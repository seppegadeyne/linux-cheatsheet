# Docker
```
docker --help
docker container --help
```

## Commands
```
docker container run [-it] [-d] image_name [command] # new container
docker container ls [-a]
docker container start container_id
docker container stop container_id
docker container exec [-it] container_id [command]
docker container commit container_id # creates new image
docker container inspect container_id
```

```
docker image ls [-a]
docker image tag image_id image_name[:tag]
docker image pul image_name[:tag]
docker image push image_name[:tag]
```

```
docker login
```

## Build Dockerfile
### Dockerfile
```
FROM alpine
RUN apk update && apk add nodejs
COPY . /app
WORKDIR /app
CMD ["node", "index.js"]
```
```
# Version: 0.0.2
FROM ubuntu:14.04
MAINTAINER Seppe Gadeyne "seppe.gadeyne@student.kdg.be"
RUN apt-get update
RUN apt-get install -y nginx
RUN echo "Hi, I am in your container" > /usr/share/nginx/html/index.html
EXPOSE 80
```      

### Create image

```
docker image build [-t image_name[:tag]] directory
docker image build -t hello:v0.1 .
docker container run hello:v0.1
docker image history fb249590dff5
docker image build -t seppegadeyne/static_web .
```

### Run 
```
docker container run [-it] [-d] image_name [command]
docker run -d -p 80:80 seppegadeyne/static_web nginx -g "daemon off;"
```

## Networking
```
docker network ls
docker network inspect bridge
```

### Bridge driver
```
docker container run -p <host_port>:<container_port> image_name [command]
docker run -it --name busybox1 busybox
```

### Host driver
Used by Google Cloud compute engine.
```
docker run --network host --name image_name [command]
docker run -it --network host --name busybox6 busybox
```

## Container in Google Cloud
### Create an instance with a container
```
gcloud compute instances create-with-container container-vm \
    --container-image=docker.io/httpd:2.4
```
## Shortcut commands
```
docker ps # docker container ls
docker images # docker image ls
docker run | start | stop | exec | commit # docker container run | start ...
docker pull | push | tag | build # docker image pull | push | tag | build ...
```

## Examples
```
docker container run hello-world
docker image pull alpine
docker image ls
docker container run alpine ls -la
docker container run -it alpine /bin/sh
docker container ls -a
docker container exec a81403cacbf0 ls
docker container run -ti ubuntu bash
docker container diff f1a22ff4d3b3
docker container commit f1a22ff4d3b3
docker image tag a9657487686f ourfiglet
docker container run ourfiglet figlet hello
docker login
docker image tag ourfiglet seppegadeyne/ourfiglet:v1.0
docker image push seppegadeyne/ourfiglet:v1.0
docker image inspect alpine
docker image inspect --format "{{ json .RootFS.Layers }}" alpine
```
