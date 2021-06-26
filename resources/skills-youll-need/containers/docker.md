---
description: These are notes of doing the cybrary Intro do Docker course.
---

# Docker

## Installation

Install docker on Debian based Linux machine \(VM or physical\)

```text
$ sudo curl -sSL https://get.docker.com/ | sh
```

or

```text
$ sudo apt-insall docker.io
```

and give your user the rights to run docker so that you don't need to sudo the docker commands.

```text
$ sudo usermod -aG docker youdockeruser
```

### 

### 

## Docker images

### Create a docker image and run it

Build a new image of ubuntu with nginx webserver capabilities. We'll name it ubuntu-nginx.

TODO

### Automate docker images

Create a directory, create a dockerfile and describe your file with the commands used in the previous section.

mkdir ubuntu-nginx \| ls

cat dockerfile &gt;

```text
FROM ubuntu 
RUN apt-get update 
RUN apt-get install -y nginx 
RUN rm -f /etc/nginx/sites-enabled/default 
COPY default /etc/nginx/sites-enabled/default 
RUN rm -f /var/www/html/index.nginx-debian.html
COPY 22.txt /var/www/html/22.txt 
COPY 33.txt /var/www/html/33.txt 
CMD ["/usr/sbin/nginx","-g","daemon off;"] 
```

Create the webserver file 

cat default &gt;

```text
    server { 
            listen 80 default_server; 
            listen [::]:80 default_server;
            
            root /var/www/html;
            index index.html; autoindex on;

    server_name localhost;

    location / {
            try_files $uri $uri/ =404;
    }
}
```

Create the files that will be copied to the web server file path

```text
touch 22.txt
touch 33.txt 
```

Build the image

```text
docker build . -t ubuntu-nginx
```

You'll see your image in the list with **docker image ls** 

Rebuild with **docker build . -t ubuntu-nginx**

Remove your image with **docker rmi ubuntu-nginx**

#### **Layers and rebuilding images**

Show the layers of the container image with 

> ```text
> $ docker image history ubuntu-nginx
> ```

If you change only one line in the dockerfile and rebuild the image, then the build is very fast because the image was built using the existing layers from the previous docker image builds. If some of these layers are being used in other containers, they can just use the existing layer instead of recreating it from scratch 

#### Run your image in a container

```text
docker container run -d --name=unginx ubuntu-nginx 
```

 Get the id of your running container and search for the IPAddress.

```text
$ docker container ls                                                                                                             
CONTAINER ID   IMAGE          COMMAND                  CREATED        STATUS        PORTS     NAMES
6c5fdc21bec2   ubuntu-nginx   "/usr/sbin/nginx -g …"   21 hours ago   Up 21 hours             unginx
529a0cfc3301   ubuntu         "/bin/bash"              23 hours ago   Up 23 hours             cranky_mendel

$ docker inspect -f "{{ .NetworkSettings.IPAddress}}" 6c5fdc21bec2
```

Insert the address into your browser.

### Make your image available to everyone on dockerhub

1. push the project to github
2. sign-in to hub.docker.com and connect the git account
3. create new reposiory with an automated build on your github project

## Docker Network

modes of networking: bridge=bridge containers together, host=directly plug the container to the hosts network adapter hosts port is directly mapped to the container, none, overlay=run over another network like VPNS used for swarms, MACVLAN=assignes a MAC address to a container making it appear as a physical device

#### Creating networks

list the networks

```text
$ docker network ls
NETWORK ID     NAME      DRIVER    SCOPE
9b9720e47fb3   bridge    bridge    local
781447733fe4   host      host      local
09f4c125bf00   none      null      local
2da32f00adf4   test1     bridge    local
536c1482cacb   test2     bridge    local
```

create networks

```text
$ docker network create test1
```

assign networks. Container should be stopped.

```text
$ docker container run -dit --net test1 --name container_on_network_test1 mycontainer
```

#### Mode: bridge \(default\)

* containers are bridged together in a network
* containers within a bridge see their names
* containers within the different networks can ping each other by IP address

#### Mode: host

* container is directly connected to the host machine
* no port mapping
* only one host network on one host
* only one container instance per port

If you start a second container with --net host then it will exit immediately as you can see in docker container ls -a

#### Mode: none

* no network
* only one instance with network=none per host

### Docker subnets

```text
$ docker network create -d bridge --subnet={subnet_IP/CIDR} networkname
```

You can modify a static configuration file in /etc/docker/daemon.json and specify your bridge networks ip range:

```text
$ cat /etc/docker/daemon.json                                                                                                       
{
        "bip": "172.105.0.1/16"
}

```

/lib/systemd/system/docker.service

### Docker port mapping

If you want a container communicate with an external system you have to map a porton the host machine with 

**docker container run -P image\_name**

docker container run -P hostport:containerport imagename

Show port mappings for container: docker port container\_id/name

Dockerfile: EXPOSE port   and run the container wit the -P flag without specifying a port \(it is already specified in the dockerfile\).

#### Example

```text
$ docker container run -ditP nginx
6a4fc5c7db4c1b7f18c3b71bc9d2a3d5cacc07254bd5fb54bc9f9afd7051490d
$ docker container ls
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS        PORTS                   NAMES
6a4fc5c7db4c   nginx     "/docker-entrypoint.…"   2 seconds ago   Up 1 second   0.0.0.0:49153->80/tcp   zealous_solomon
4cba5ebf548c   nginx     "/docker-entrypoint.…"   11 hours ago    Up 11 hours   80/tcp                  nostalgic_gauss
$ curl localhost:49153
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
```

## Docker Storage handling

#### Volumes

 /var/lib/docker/volumes, managed by docker, persistent

best for: cloud options, for backup and restore

#### Bind mounts

managed by the host system, absolut path has to be used, can reference to non existing data, less performant, security risks

best for: sharing files between containers and host system, sharing of files that need to be shared with the host, files and directories with a consistent structure 

#### tmpfs

non persistant, in the RAM

best for: sensitive information such as credentials

### Example with volumes

Create a volume

```text
$ docker volume create --name volume_1
volume_1

$ docker volume ls
DRIVER    VOLUME NAME
local     volume_1

$ docker volume inspect volume_1
[
    {
        "CreatedAt": "2021-04-29T08:59:10+01:00",
        "Driver": "local",
        "Labels": {},
        "Mountpoint": "/var/lib/docker/volumes/volume_1/_data",
        "Name": "volume_1",
        "Options": {},
        "Scope": "local"
    }
]
```

Assign the volume

```text
$ docker container run -ditv volume_1:/vol1 nginx                                                                                   
9a533eb54fa11c1ff4fa5184c43fe398a6c44167c4f56bd6b23d65fec051fdf9
```

Use the volume, create a file

```text
$ docker container exec -it container_name /bin/bash
root@9a533eb54fa1:/# ls
bin   dev                  docker-entrypoint.sh  home  lib64  mnt  proc  run   srv  tmp  var
boot  docker-entrypoint.d  etc                   lib   media  opt  root  sbin  sys  usr  vol1
root@9a533eb54fa1:/# cd vol1/
root@9a533eb54fa1:/vol1# touch 22.txt
```

Create a container and assign the volume as read-only 

```text
$ docker container run -ditv volume_1:/vol1:ro nginx
9c33da20593119dfbf0610df86eae6149b5c53673fa53041c6e7025f549ba898
```

Test that it's readonly

```text
$ docker container exec -it intelligent_noyce /bin/bash                                                               
root@9c33da205931:/# ls
bin   dev                  docker-entrypoint.sh  home  lib64  mnt  proc  run   srv  tmp  var
boot  docker-entrypoint.d  etc                   lib   media  opt  root  sbin  sys  usr  vol1
root@9c33da205931:/# cd vol1/
root@9c33da205931:/vol1# touch 33
touch: cannot touch '33': Read-only file system
```

### Example with bind mount

Mount a volume /home/username/vol-h to vol-1 of the container

```text
$ docker container run -dit --mount 'type=bind,src=/home/username/vol-h,dst=/vol-h' nginx                        
38eb1e7928499b30c340f2e700d444fc52f42b6e234916ad1941f0d63e045639

```

Verify that the volume has been mounted

```text
$ docker container inspect ecstatic_sammet | grep vol-h
                    "Source": "/home/username/vol-h",
                    "Target": "/vol-h"
                "Source": "/home/username/vol-h",
                "Destination": "/vol-h",                                                                                     
```

Now on the host machine I create a file

```text
$ touch vol-h/123.txt 
```

Which is accessible in the container

```text
$ docker container exec -it ecstatic_sammet /bin/bash
root@38eb1e792849:/# ls vol-h/
123.txt
```

Another way is to map a non existing volume, docker creates it, but it will not be accessible without root permissions by the  host system

```text
$ docker container run -dit -v /home/martina/vol-h-1:/vol-h-1 nginx                                                                 
6d879e994b9c8ca329c7ddeb7f1258bbf814e1c673b27d622d90f9cca3b65827
```

### Example with tmpfs

No src in the create command. Destination is the mount point on the container. Create a container with a 

```text
$ docker container run -dit --mount type=tmpfs,dst=/tmph nginx
c9e6c97b0c13981e330eaa9bede29009c39cdf642d473a45deb723cf7c703031
```

### Map volumes within dockerfile

Create dockerfile: 

```text

```

build the image

```text
$ docker build . -t ubnginx-cop
```

run a container with the image, --mount must be specified

```text
 $ docker container run -dit --mount 'src=copyfiles,dst=/copyfiles' 
```

now we see that everything from the destination volumes gets copied onto the source volume.

```text
$ sudo ls /var/lib/docker/volumes/copyfiles/_data
22.txt  33.txt
```

you can remove the files on the host machine.

## Docker orchestration: compose

Is a tool for managing multiple containers. Uses a YAML file for configuration. Start/stop multiple containers with one command line. It also is a single documentation of an entire environment in a single file. It's easier to move services between environments.

### Installation

Two commands, see [https://docs.docker.com/compose/install/](https://docs.docker.com/compose/install/)

### Get your services running

Create the dockerfile **docker-compose.yml** for a simple voting app [https://github.com/dockersamples/example-voting-app/blob/master/docker-compose.yml](https://github.com/dockersamples/example-voting-app/blob/master/docker-compose.yml)  and add this content to it:

```text
$ cat docker-compose.yml                                                                                                            
version: "3.9"
services:
  redis:
    image: redis
    ports: ["6379"]
  db:
    image: postgres:9.4
  vote:
    image: dockersamples/examplevotingapp_vote:before
    ports:
      - "5000:80"
  result:
    image: dockersamples/examplevotingapp_result:before
    ports:
      - "5001:80"
  worker:
    image: dockersamples/examplevotingapp_worker
```

run it with **docker-compose up** then check if the web app is running using localhost:5000 and localhost 5001 in your browser.

Now you can add other other to the compose file. Documentation here: [https://docs.docker.com/compose/](https://docs.docker.com/compose/)

## Docker UI: portainer

Is a UI for managing docker containers.

Installation

```text
$  docker container run -d -p 9000:9000 -p 8000:8000 --name portainer --restart unless-stopped --mount 'type=bind,src=/var/run/docker.sock,dst=/var/run/docker.sock' --mount 'src=portainer_data,dst=/data' portainer/portainer
```

Get Ipaddress and log in. I use grep to get the ip address, instead of format

```text
$ docker container inspect 7e8c467786c2 | grep IPAddress                                                                            
            "SecondaryIPAddresses": null,
            "IPAddress": "172.105.0.2",
                    "IPAddress": "172.105.0.2",
```

Ok, I open a browser using 172.105.0.2:9000

## Docker local registry

[https://docs.docker.com/registry/deploying/](https://docs.docker.com/registry/deploying/)

## Docker Reference

### Basic Docker commands

`docker container run <containerID or name>`

`docker ls -a (for all running and non running containers)`

`docker stats --no-stream (single output of the stats)`

`docker stats --no-stream --no-trunc (output with IDs and other info without truncating)`

#### Modes

* -i interacive mode
* -d deamon
* -t terminal allocation

docker container run -idt --name=&lt;containername&gt; &lt;imagename&gt;

### Useful inspects

Getting IPAddress

```text
$ docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' ubnginx-t2-1 
```

json format of containers in the network network1

```text
$ docker network inspect -f '{{json .Containers}}' network1
```

 or use grep:

```text
$ docker container inspect container_name | grep "IPAddress"
```

inspect a specific container

```text
$ docker container inspect vigilant_fermat_container_name
```

### Helpful docker resources

Docker Desktop for Windows [https://hub.docker.com/editions/community/docker-ce-desktop-windows](https://hub.docker.com/editions/community/docker-ce-desktop-windows)

Docker Toolbox [https://github.com/docker/toolbox/releases](https://github.com/docker/toolbox/releases) 6. Docker Hub [https://hub.docker.com/](https://hub.docker.com/) 

Talk to Docker [https://docs.docker.com/engine/reference/commandline/docker/](https://docs.docker.com/engine/reference/commandline/docker/) 

Docker Compose [https://github.com/docker/compose/releases](https://github.com/docker/compose/releases) 

Docker Compose Cheat Sheet [https://jstobigdata.com/docker-compose-cheatsheet/](https://jstobigdata.com/docker-compose-cheatsheet/) 

Docker Docs [https://docs.docker.com/](https://docs.docker.com/)

### More labs

Online labs [https://labs.play-with-docker.com/](https://labs.play-with-docker.com/)

