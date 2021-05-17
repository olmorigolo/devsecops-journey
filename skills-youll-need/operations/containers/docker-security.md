# Docker security

## Attack vectors

Docker can be attacked on these vectors:

1. OS and kernel attacks
2. Network-based attacks
3. Daemon-based attacks
4. Image-based attacks
5. Application-based attacks.

## Footprinting

### Find out which host has a docker deamon running:

```text
ps aux | grep dockerd
```

then output the running containers \(-a also exited containers\)

```text
$ docker ps 
```

### **Find out if a container leaks information in the env config**

AIf the container had been started with docker environment variables, these vars can leak information.

Example:

> ```text
> $ docker run -it --rm -e \ USERNAME=user -e PASSWORD=secretpassword ubuntu-nginx sh
> ```

If you have access to docker container, you can read all of its environment variables by typing the env command.

```text
# env
HOSTNAME=4aa2989f5194
HOME=/root
PKG_RELEASE=1~buster
TERM=xterm
NGINX_VERSION=1.19.10
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
NJS_VERSION=0.5.3
PWD=/
PASSWORD=secretpassword
# 

```

### Find out if memory had been allocated to a container

Verify if a particular container is suspectable for a Denial of service attack. If memory has been allocated, the container might be vulnerable to a DoS attack.

Example:

```text
$ docker run --rm -it -m 100mb -d alpine sh
```

Show the memory allocation in the stats:

```text
$ docker stats a3cba64a7c7884ca78f5c2a1c4b925fbe0534a233d51537ae44cb3a4b34b47d0
```

```text
CONTAINER ID   NAME             CPU %     MEM USAGE / LIMIT   MEM %     NET I/O     BLOCK I/O     PIDS
a3cba64a7c78   focused_cannon   0.00%     532KiB / 100MiB     0.52%     796B / 0B   1.11MB / 0B   1

```

## Attack docker

{% embed url="https://www.practical-devsecops.com/lesson-4-hacking-containers-like-a-boss/" %}

{% embed url="https://www.practical-devsecops.com/lesson-5-hacking-containers-like-a-boss-part-2/" %}

## Defend docker

{% embed url="https://www.practical-devsecops.com/lesson-6-defending-container-infrastructure/" %}



