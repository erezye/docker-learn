# general notes

Docker - tips

show all images
```bash
docker images
```
``show all running containers``

x

running a container with “terminal integration” (-ti) ,see the ubuntu version, and exit
```bash
docker run -ti ubuntu:latest bash
root@e6eff42b9102:/# cat /etc/lsb-release
root@e6eff42b9102:/# exit or ctrl d
```


``Stopped containers``:

First, let’s see all the stopped containers:

```bash
docker ps -a //for all
docker ps -l //for the last stoped container
```

How to create an image from a container?
Docker commit takes containers and creates a new image from them:
1. run a container
2. create file inside the container
3. exit
4. get the last container exits
5. docker commit ``<source docker> <new images name>``
6. check image exists
7. run a cointainer with the new image craeted
8. see the new file inside the new container
 ```bash
docker images
docker run -ti ubuntu:latest bash
root@e6eff42b9102:/# cat /etc/lsb-release
root@e6eff42b9102:/# exit or ctrl d
docker ps -a #for all
docker ps -l #for the last stoped container
docker run -ti ubuntu bash
touch My-sample-state-file
exit
docker ps -l
# CONTAINER
# ID   IMAGE     COMMAND   CREATED
# TS     NAMES
# 9dd0d5d7240c   ubuntu    "bash"
# seconds ago
# STATUS
# 35 seconds ago
# POR
#           optimistic_albattani
docker commit optimistic_albattani my-custome-ubuntu
docker images
docker run -ti my-custome-ubuntu bash
ls
# My-sample-state-file  bin  boot  dev
# etc  home  lib  lib32  lib64  libx32
# media  mnt  opt  proc  root  run
# sbin  srv  sys  tmp  usr  var
exit
Exited (0) 2
```


``pull and push images``
```bash
docker pull debian:sid
docker tag debian:sid erezye/my-debian:v11
docker push debian:sid erezye/my-debian:v11
```




Remove unused data https://docs.docker.com/engine/reference/commandline/system_prune/

```bash
docker system prune [OPTIONS]
```

## Registry
there is a docker build in as part of the docker, registry is a network service

what should we do to create local registry

``` bash
docker run -d -p 5000:5000 --restart=always --name registry registry:2
# -d deattached
# -p port forword
# restart=always if this containers dies just restart it
# call is registry from registry version 2 image
```
push to local regitry
``` bash
# tag the image first, MUST with the local:5000 for local registry pushing
docker tag ubuntu:16.04 localhost:5000/cx/myubuntu:19.99
#push the image to the local registry
docker push localhost:5000/cx/myubuntu:19.99
```

how to save images to archive (tar.gz) great for backups and sending images


``` bash
# save 3 images (google-is-big:latest alpine  ubuntu) into one file my-images.tar.gz
docker save -o my-images.tar.gz google-is-big:latest  ubuntu:16.04

docker rmi google-is-big:latest ubuntu:16.04

docker load -i my-images.tar.gz
```

