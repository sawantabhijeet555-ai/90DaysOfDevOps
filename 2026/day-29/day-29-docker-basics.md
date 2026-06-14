What is a container ???

"A container is a lightweight, isolated environment that packages an application along with all its dependencies, libraries, and runtime, ensuring that the application runs consistently across different environments."

Why do we need containers?

Consistency: Eliminates the "works on my machine" problem by providing the same environment everywhere.
Portability: Containers can run on any system that has a container runtime like Docker.
Isolation: Multiple applications can run independently without affecting each other.
Efficient resource usage: Containers share the host OS kernel, making them lighter and faster than virtual machines.
Scalability: They enable easy deployment and scaling, especially in platforms like Kubernetes.
Simplified CI/CD: Containers make application packaging and deployments more reliable and repeatable.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
Containers vs Virtual Machines — what's the real difference?

The main difference is that virtual machines virtualize hardware and require a separate guest OS for each instance, whereas containers virtualize the operating system and share the host OS kernel. Because of this, containers are lightweight, start in seconds, and use fewer resources, making them ideal for modern cloud-native applications.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
What is the Docker architecture? (daemon, client, images, containers, registry)

Docker uses a client-server architecture. The Docker client sends commands to the Docker daemon, which manages images, containers, networks, and volumes. Images are templates used to create containers, and registries such as Docker Hub or AWS ECR are used to store and distribute images. This architecture enables consistent and portable application deployment across different environments.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
## Task 2: Install Docker
# Install Docker on your machine (or use a cloud instance)
# Verify the installation
# Run the hello-world container
# Read the output carefully — it explains what just happened


ubuntu@ip-172-31-14-118:~$ sudo apt-get install docker.io

ubuntu@ip-172-31-14-118:~$ docker --version
Docker version 29.1.3, build 29.1.3-0ubuntu4.1

ubuntu@ip-172-31-14-118:~$ sudo usermod -aG docker $USER
ubuntu@ip-172-31-14-118:~$ newgrp docker
ubuntu@ip-172-31-14-118:~$ docker run "hello-world"
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
4f55086f7dd0: Pull complete 
d5e71e642bf5: Download complete 
Digest: sha256:96498ffd522e70807ab6384a5c0485a79b9c7c08ca79ba08623edcad1054e62d
Status: Downloaded newer image for hello-world:latest

Hello from Docker!

Step 1: Docker Client Receives the Command
       The Docker Client sent this request to the Docker Daemon (dockerd).
       
       You
        ↓
    Docker Client
        ↓
    Docker Daemon

Step 2: Docker Checks for the Image Locally
       Docker first searched your machine for the image:
       -> Unable to find image 'hello-world:latest' locally

Step 3: Docker Pulls the Image from Docker Hub
        Docker automatically contacted Docker Hub and downloaded the image. 

Step 4: Image Gets Stored Locally
        Now the image exists on your machine.

Step 5: Docker Creates a Container
        From the image, Docker created a container:

Step 6: Container Executes /hello
       Inside the image is a tiny program:
       which printed:

                    Hello from Docker!

Step 7: Program Finishes
        Since the hello-world program only prints a message and exits, the container stops immediately.


When we execute docker run hello-world, the Docker client sends the request to the Docker daemon. The daemon checks whether the image exists locally; if not, it pulls the image from Docker Hub, stores it locally, creates a container from that image, runs the /hello executable inside the container, displays the message, and finally stops the container because the process has completed.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------

## Task 3: Run Real Containers

# Run an Nginx container and access it in your browser
ubuntu@ip-172-31-14-118:~$ docker pull nginx

ubuntu@ip-172-31-14-118:~$ docker run -d -p 80:80 --name test-nginx nginx
556b77bc553515a288733b3e76625a21847cc002e6d7d042b79bbbfd4379d316
ubuntu@ip-172-31-14-118:~$ docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS                                 NAMES
556b77bc5535   nginx     "/docker-entrypoint.…"   8 seconds ago   Up 7 seconds   0.0.0.0:80->80/tcp, [::]:80->80/tcp   test-nginx

# Run an Ubuntu container in interactive mode — explore it like a mini Linux machine
ubuntu@ip-172-31-14-118:~$ docker pull ubuntu
Using default tag: latest
latest: Pulling from library/ubuntu
1c24335ddd46: Pull complete 
6f5c5aa4e145: Pull complete 
9bcf140d7f0f: Download complete 
Digest: sha256:f3d28607ddd78734bb7f71f117f3c6706c666b8b76cbff7c9ff6e5718d46ff64
Status: Downloaded newer image for ubuntu:latest
docker.io/library/ubuntu:latest
ubuntu@ip-172-31-14-118:~$ docker run -it --name test-ubuntu ubuntu
root@93a8c62e6ede:/# 

# List all running containers
ubuntu@ip-172-31-14-118:~$ docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS                                 NAMES
556b77bc5535   nginx     "/docker-entrypoint.…"   5 minutes ago   Up 5 minutes   0.0.0.0:80->80/tcp, [::]:80->80/tcp   test-nginx

# List all containers (including stopped ones)
ubuntu@ip-172-31-14-118:~$ docker ps -a
CONTAINER ID   IMAGE     COMMAND                  CREATED              STATUS                      PORTS                                 NAMES
93a8c62e6ede   ubuntu    "/bin/bash"              About a minute ago   Exited (0) 31 seconds ago                                         test-ubuntu
556b77bc5535   nginx     "/docker-entrypoint.…"   6 minutes ago        Up 6 minutes                0.0.0.0:80->80/tcp, [::]:80->80/tcp   test-nginx

# Stop and remove a container
ubuntu@ip-172-31-14-118:~$ docker stop 93a8c62e6ede 556b77bc5535
93a8c62e6ede
556b77bc5535
ubuntu@ip-172-31-14-118:~$ docker rm 93a8c62e6ede 556b77bc5535
93a8c62e6ede
556b77bc5535

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
Task 4: Explore
Run a container in detached mode — what's different?

The main difference is that without -d, the container runs in the foreground and attaches to the terminal, whereas with -d, the container runs in the background and the terminal is immediately available for other commands. Detached mode is typically used for long-running services like Nginx, MySQL.
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
Give a container a custom name

-> docker run --name <Custom_Name> <Image_Name>

-> e.g. docker run -d -p 80:80 --name test-nginx nginx
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------

Map a port from the container to your host

->docker run -p <host-port>:<container-port> <image>
-> e.g. docker run -d -p 8080:80 --name test-nginx nginx

Port mapping exposes an application running inside a container to the outside world. We use the -p host_port:container_port option. For example, docker run -d -p 8080:80 nginx maps port 8080 on the host to port 80 inside the container, allowing external users to access the Nginx service.
----------------------------------------------------------------------------------------------------------------------------------------------------------------------- 

Check logs of a running container

docker logs -f my-nginx

We use the docker logs command to view the standard output and error streams of a container. The -f option allows us to follow logs in real time, which is useful for monitoring and troubleshooting containerized applications.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------

Run a command inside a running container

docker exec [OPTIONS] <container-name-or-id> <command>

We use the docker exec command to run commands inside a running container. The -it option allows us to open an interactive shell and troubleshoot or inspect the container.

ubuntu@ip-172-31-14-118:~$ docker exec 24c9e412fc97 ls

ubuntu@ip-172-31-14-118:~$ docker exec -it 24c9e412fc97 bash
root@24c9e412fc97:/# ls
bin  boot  dev  docker-entrypoint.d  docker-entrypoint.sh  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------

