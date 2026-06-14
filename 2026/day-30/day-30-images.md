## Task 1: Docker Images
# Pull the nginx, ubuntu, and alpine images from Docker Hub

ubuntu@ip-172-31-14-118:~$ docker pull ubuntu
ubuntu@ip-172-31-14-118:~$ docker pull nginx
ubuntu@ip-172-31-14-118:~$ docker pull alpine

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
# List all images on your machine — note the sizes

ubuntu@ip-172-31-14-118:~$ docker images
                                                                  
IMAGE                ID             DISK USAGE   CONTENT SIZE   EXTRA
alpine:latest        a2d49ea686c2       13.1MB         3.95MB        
hello-world:latest   96498ffd522e       25.9kB         9.49kB        
nginx:latest         608a100c7165        241MB           66MB        
ubuntu:latest        f3d28607ddd7        160MB         45.3MB    

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
# Compare ubuntu vs alpine — why is one much smaller?

Alpine Linux is a minimal distribution designed specifically for containers. It contains only essential packages and uses musl libc, whereas Ubuntu includes many utilities and uses glibc, making it significantly larger.

For example:

Ubuntu: ~70–80 MB
Alpine: ~5–10 MB

Because of its small size, Alpine provides:

Faster image downloads
Reduced storage requirements
Smaller attack surface
Faster deployments

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
# Inspect an image — what information can you see?

ubuntu@ip-172-31-14-118:~$ docker inspect a2d49ea686c2

docker inspect provides detailed metadata about an image or container.

It provides information such as:

#Image ID
#Creation date
#Operating system
#Architecture
#Environment variables
#Entrypoint and default command
#Labels
#Layers used to build the image

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
# Remove an image you no longer need

docker stop <Image_Name> && docker rm <Image_Name>

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
## Task 2: Image Layers
# Run docker image history nginx — what do you see?

docker history <Image-Name>

# Each line is a layer. Note how some layers show sizes and some show 0B
# Write in your notes: What are layers and why does Docker use them?


It shows the layers that make up the Nginx image. Docker images are built layer by layer, and each instruction in the Dockerfile creates a new layer.

Understanding the Output
IMAGE          CREATED      CREATED BY                     SIZE
608a100c7165   3 days ago   CMD ["nginx" "-g" "daemon off;"]   0B

Layer 1: CMD ["nginx","-g","daemon off;"]

Default command executed when the container starts.
daemon off; keeps Nginx running in the foreground.
Size = 0B because commands themselves don't add files.

Layer 2: Stop Signal
STOPSIGNAL SIGQUIT

Defines which signal Docker sends when stopping the container.
Allows Nginx to shut down gracefully.
No additional storage → 0B.

EXPOSE map[80/tcp:{}]
Layer 3: Exposed Port
EXPOSE 80

Documents that Nginx listens on port 80.
Doesn't actually publish the port.
Size = 0B.

ENTRYPOINT ["/docker-entrypoint.sh"]
Layer 4: Entrypoint
ENTRYPOINT ["/docker-entrypoint.sh"]

Script executed whenever the container starts.
Initializes configuration before launching Nginx.
Size = 0B.

Layer 5–9: COPY Commands

Example:

COPY docker-entrypoint.sh /
COPY 10-listen-on-ipv6-by-default.sh
COPY 15-local-resolvers.envsh
COPY 20-envsubst-on-templates.sh
COPY 30-tune-worker-processes.sh

These files are copied into the image.

They are startup scripts responsible for:

IPv6 support
DNS resolver settings
Environment variable substitution
Worker process tuning

Sizes are small:

8.19kB
12.3kB
16.4kB

Layer 10: Main Installation Layer
RUN /bin/sh -c set -x && groupadd ...
SIZE: 87.1MB

This is the most important layer.

Equivalent to:

RUN apt-get update
RUN apt-get install nginx

This layer contains:

Nginx binaries
Debian packages
Libraries
User and group creation

It occupies 87.1 MB, which forms most of the image size.

Environment Variable Layers

Example:

ENV NGINX_VERSION=1.31.1
ENV NJS_VERSION=0.9.9
ENV ACME_VERSION=0.4.1

These are metadata variables.

Equivalent Dockerfile:

ENV NGINX_VERSION=1.31.1
ENV NJS_VERSION=0.9.9

They consume no additional disk space (0B).

LABEL Layer
LABEL maintainer=NGINX Docker Maintainers

Metadata about image maintainers.

Size:

0B
Base Layer
# debian.sh --arch 'amd64' out/ 'trixie'
SIZE: 87.4MB

This is the Debian base image.

Everything else is built on top of it.

Debian (87.4 MB)
       ↓
Install Nginx (87.1 MB)
       ↓
Copy scripts
       ↓
Entrypoint
       ↓
Expose port 80
       ↓
CMD nginx -g "daemon off;"
Why do many rows show <missing>?

<missing> simply means:

Intermediate layers don't have their own image IDs.
Docker only assigns an ID to the final image:
608a100c7165

These layers still exist and are reused for caching.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
## Task 3: Container Lifecycle
# Practice the full lifecycle on one container:

# Create a container (without starting it)
# Start the container
# Pause it and check status
# Unpause it
# Stop it
# Restart it
# Kill it
# Remove it
# Check docker ps -a after each step — observe the state changes.

ubuntu@ip-172-31-14-118:~$ docker create --name test-nginx nginx:latest
9069a5737102aad571fb87174b5c65fc0c9efe4abaf7aa9f6ecd43b438072fe4
ubuntu@ip-172-31-14-118:~$ docker ps -a
CONTAINER ID   IMAGE          COMMAND                  CREATED              STATUS    PORTS     NAMES
9069a5737102   nginx:latest   "/docker-entrypoint.…"   About a minute ago   Created             test-nginx
ubuntu@ip-172-31-14-118:~$ docker start test-nginx
test-nginx
ubuntu@ip-172-31-14-118:~$ docker ps -a
CONTAINER ID   IMAGE          COMMAND                  CREATED              STATUS         PORTS     NAMES
9069a5737102   nginx:latest   "/docker-entrypoint.…"   About a minute ago   Up 4 seconds   80/tcp    test-nginx
ubuntu@ip-172-31-14-118:~$ docker pause test-nginx
test-nginx
ubuntu@ip-172-31-14-118:~$ docker ps -a
CONTAINER ID   IMAGE          COMMAND                  CREATED         STATUS                       PORTS     NAMES
9069a5737102   nginx:latest   "/docker-entrypoint.…"   2 minutes ago   Up About a minute (Paused)   80/tcp    test-nginx
ubuntu@ip-172-31-14-118:~$ docker unpause test-nginx
test-nginx
ubuntu@ip-172-31-14-118:~$ docker ps -a
CONTAINER ID   IMAGE          COMMAND                  CREATED         STATUS         PORTS     NAMES
9069a5737102   nginx:latest   "/docker-entrypoint.…"   3 minutes ago   Up 2 minutes   80/tcp    test-nginx
ubuntu@ip-172-31-14-118:~$ docker stop test-nginx
test-nginx
ubuntu@ip-172-31-14-118:~$ docker ps -a
CONTAINER ID   IMAGE          COMMAND                  CREATED         STATUS                     PORTS     NAMES
9069a5737102   nginx:latest   "/docker-entrypoint.…"   4 minutes ago   Exited (0) 4 seconds ago             test-nginx
ubuntu@ip-172-31-14-118:~$ docker restart test-nginx
test-nginx
ubuntu@ip-172-31-14-118:~$ docker ps -a
CONTAINER ID   IMAGE          COMMAND                  CREATED         STATUS         PORTS     NAMES
9069a5737102   nginx:latest   "/docker-entrypoint.…"   4 minutes ago   Up 4 seconds   80/tcp    test-nginx
ubuntu@ip-172-31-14-118:~$ docker kill test-nginx
test-nginx
ubuntu@ip-172-31-14-118:~$ docker ps -a
CONTAINER ID   IMAGE          COMMAND                  CREATED         STATUS                       PORTS     NAMES
9069a5737102   nginx:latest   "/docker-entrypoint.…"   5 minutes ago   Exited (137) 3 seconds ago             test-nginx
ubuntu@ip-172-31-14-118:~$ docker rm test-nginx
test-nginx
ubuntu@ip-172-31-14-118:~$ docker ps -a
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
## Task 4: Working with Running Containers
# Run an Nginx container in detached mode

ubuntu@ip-172-31-14-118:~$ docker run -d -p 80:80 --name test-nginx nginx
188c083952c0fd4c89dea3d2d01817a002d5f6f6d497ea2d479611c776937e5e
ubuntu@ip-172-31-14-118:~$ docker ps -a
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS                                 NAMES
188c083952c0   nginx     "/docker-entrypoint.…"   7 seconds ago   Up 6 seconds   0.0.0.0:80->80/tcp, [::]:80->80/tcp   test-nginx

# View its logs

ubuntu@ip-172-31-14-118:~$ docker logs 188c083952c0

# View real-time logs (follow mode)

ubuntu@ip-172-31-14-118:~$ docker logs -f 188c083952c0

# Exec into the container and look around the filesystem

ubuntu@ip-172-31-14-118:~$ docker exec -it test-nginx bash
root@188c083952c0:/# ls
bin  boot  dev  docker-entrypoint.d  docker-entrypoint.sh  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var

# Run a single command inside the container without entering it

ubuntu@ip-172-31-14-118:~$ docker exec -it test-nginx bash -c "pwd && ls -l"
/
total 64
lrwxrwxrwx   1 root root    7 May  8 16:10 bin -> usr/bin
drwxr-xr-x   2 root root 4096 May  8 16:10 boot
drwxr-xr-x   5 root root  340 Jun 14 18:03 dev
drwxr-xr-x   1 root root 4096 Jun 11 00:23 docker-entrypoint.d
-rwxr-xr-x   1 root root 1620 Jun 11 00:23 docker-entrypoint.sh
drwxr-xr-x   1 root root 4096 Jun 14 18:03 etc
drwxr-xr-x   2 root root 4096 May  8 16:10 home
lrwxrwxrwx   1 root root    7 May  8 16:10 lib -> usr/lib
lrwxrwxrwx   1 root root    9 May  8 16:10 lib64 -> usr/lib64
drwxr-xr-x   2 root root 4096 Jun 10 00:00 media
drwxr-xr-x   2 root root 4096 Jun 10 00:00 mnt
drwxr-xr-x   2 root root 4096 Jun 10 00:00 opt
dr-xr-xr-x 181 root root    0 Jun 14 18:03 proc
drwx------   1 root root 4096 Jun 14 18:11 root
drwxr-xr-x   1 root root 4096 Jun 14 18:03 run
lrwxrwxrwx   1 root root    8 May  8 16:10 sbin -> usr/sbin
drwxr-xr-x   2 root root 4096 Jun 10 00:00 srv
dr-xr-xr-x  13 root root    0 Jun 14 18:03 sys
drwxrwxrwt   2 root root 4096 Jun 10 00:00 tmp
drwxr-xr-x   1 root root 4096 Jun 10 00:00 usr
drwxr-xr-x   1 root root 4096 Jun 10 00:00 var

# Inspect the container — find its IP address, port mappings, and mounts

ubuntu@ip-172-31-14-118:~$ docker inspect test-nginx

"NetworkSettings": {
            "SandboxID": "79497f151fa0cd06668f1756dabed21d7462b8f30fe054c6c3f90ced00270ac5",
            "SandboxKey": "/var/run/docker/netns/79497f151fa0",
            "Ports": {
                "80/tcp": [
                    {
                        "HostIp": "0.0.0.0",
                        "HostPort": "80"
                    },
                    {
                        "HostIp": "::",
                        "HostPort": "80"
                    }
                ]
            },
            "Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "DriverOpts": null,
                    "GwPriority": 0,
                    "NetworkID": "a8d53b531bc7ed39e04127034e232f4f47918a624e52b1d9a17230e759629d6e",
                    "EndpointID": "7ad67e700260960911f293c5eedd7495a540c029dd946e97a35567b19ce95eeb",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.2",
                    "MacAddress": "d2:42:59:cf:cd:78",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "DNSNames": null
                }
            }
        },

----------------------------------------------------------------------------------------------------------------------------------------------------------------------
## Task 5: Cleanup

# Stop all running containers in one command

ubuntu@ip-172-31-14-118:~$ docker stop $(docker ps -q)
188c083952c0


# Remove all stopped containers in one command

ubuntu@ip-172-31-14-118:~$ docker container prune

WARNING! This will remove all stopped containers.
Are you sure you want to continue? [y/N] y
Deleted Containers:
aac4dba9710c19f2858c1931380b97ff93cf257bd6f7529eeae8d028ede16f5e
e1ecb0f72eab0dc7066c9d4a2aa470ae6ab5bb8e5a69e6b387c2788847603739
188c083952c0fd4c89dea3d2d01817a002d5f6f6d497ea2d479611c776937e5e

Total reclaimed space: 94.21kB 


# Remove unused images
ubuntu@ip-172-31-14-118:~$ docker image prune -a 


# Check how much disk space Docker is using

ubuntu@ip-172-31-14-118:~$ docker system df

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------


