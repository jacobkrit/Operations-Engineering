# Docker

## Docker Linux (Server) Installation
1. Follow the official installation guidelines ([Docker Linux Installation](https://docs.docker.com/engine/install/debian/)) 
2. Verify the successful installation: `sudo docker run hello-world`
3. Better practice is to execute docker as a **non-root user**. When docker is installed a docker group usually is created which grants root-level privileges to the users added. As we discussed at the `Linux-User-Group-Administration.md` we need:
    - check if a docker group exists: `cat /etc/group | grep docker` (if doesn't exist create one: `sudo groupadd docker`)
    - check the users which currently are members of the docker group: `groups docker` (if it's a new docker installation it should be empty)
    - add users to it: `sudo usermod -aG docker [USERNAME]` ([Manage Docker as a non-root user](https://docs.docker.com/engine/install/linux-postinstall/))
    - restart the server
    - verify the execution without sudo privileges: `docker run hello-world`


## Basic Docker Commands

| Command  | Description|
|---|---| 
|```docker --help```| Prints all the coomands basic commands |  
|`docker --version [OPTIONS]`| Get the current version of the docker |  
|`docker images`|Prints all the current images installed locally.|  
|`docker pull [OPTIONS] [IMAGE_NAME]`|Pull an image/repository from a registry|  
|`docker run [OPTIONS] [IMAGE_NAME]`|Create a container from an image (a new each time)|  
|`docker ps`| List all the containers currently running (plotting their IDs, Ports etc)|  
|`docker ps -a`|print all the past executed containers, the history|  
|`docker container ls -a`|print all the past executed containers, the history|  
|`docker stop [OPTIONS] [Container_ID]`|stop one or more running containers|  
|`docker restart [OPTIONS] [Container_ID]`|restart one or more containers|  
|`docker start [OPTIONS] [Container_ID]`|restart one or more containers|  
|`docker kill [OPTIONS] [Container_Name]`| kill one or more containers|  
|`docker logs [Container_ID]`|helps debugging|  
|`docker exec -it Container_ID COMMAND`|run a command in a running container|  
|`docker exec -it Container_ID /bin/bash`|get the bash as vm as a root user|  
|`docker commit`|create a new image from the container image|  
|`docker push`|push an image or repository to a registry|  
|`docker network ls`|Available networks, used for intercommunication between containers|  
|`docker network create NETWORK_NAME`|create a docker network|  
|` `| |  

There are two categories of commands (basic commands & management commands) in management commands arguments (options) are required

![docker-commands](img/docker-commands.jpg "docker-commands")

<img src="images/docker-commands.jpg" alt="docker-commands" width="500px">

Alternative Docker commands:

| Command  | Command| Description|
|---|---|---|
|`docker container ls`|`docker ps`| list of processes | 
|`docker builder build `|`docker build`| build a new image |
|`docker container inspect`|`docker inspect`| inspect specific container |
|`docker container commit`|`docker commit`| create image on specific container|
|`docker container run`|`docker run`| start a new container |
 

 

# Docker Architecture

Docker uses a Client/Server Architecture. The Docker Client (AKA **docker**) talks to the Docker Server/Host (AKA **daemon**):
- **Client/docker**: is the primary way that users interact with Docker. By using commands (example: `docker pull [OPTIONS] [IMAGE_NAME]`) the client sends these requests to Server/daemon, which carries them out. The Docker Client can communicate with more than one Server/daemon.
- **Server/daemon**: manages objects such as images, containers, networks, and volumes. Basically daemon does the heavy lifting of building, running, and distributing your Docker containers. A daemon can also communicate with other daemons to manage Docker services.
- Client and Server/daemon can run on the same system, or you can connect a Docker client to a remote Docker Server/daemon. 
- Client and Server/daemon communicate using a **REST API**, over UNIX sockets or a network interface.
- **Docker Compose** is a type of Client, that lets you work with applications consisting of a set of **containers**.
- **Registry**: stores Docker **images**. Docker Hub is a public registry, and Docker is configured to look for images on Docker Hub by default. By using commands such as `docker pull [OPTIONS] [IMAGE_NAME]` or `docker run [OPTIONS] [IMAGE_NAME]`, the required images are pulled from your configured registry. When you use the `docker push` command, your image is pushed to your configured registry.

<img src="images/docker-architecture.jpg" alt="docker-architecture" width="500px">

## Docker objects

- **Containers**: is an **isolated** place where an **application** runs without affecting the rest of the system and without the system impacting the application. Because they are isolated, containers are well-suited for securely running software like databases or web applications that need access to sensitive resources without giving access to every user on the system.
    - Containers run natively on Linux and shares the host machine’s kernel, they are **lightweight**, not using more memory than other executables. If you stop a container, it will not automatically restart unless you configure it that way. Containers can be much **more efficient** than virtual machines because they don’t need the overhead of an entire operating system. They share a single kernel with other containers and boot in seconds instead of minutes.
    - Use containers for **packaging an application** with all the components it needs, then ship it all out as one unit. This approach is popular because it eliminates the friction between development, QA, and production environments, enabling faster software shipping. Building and deploying applications inside software containers eliminates “works on my machine” problems when collaborating on code with fellow developers.
    - You can **create**, **start**, **stop**, **move**, or **delete** a container using the **Docker API** or CLI. You can connect a container to one or more networks, attach storage to it, or even create a new image based on its current state.

- **Images**: is a read-only template with instructions for creating a Docker container. Think of an image like a **blueprint** or snapshot of what will be in a container when it runs. A Docker image executes code in a Docker container.  
    - Docker **container as a running image instance**. You can create many containers from the same image, each with its own unique data and state.
    - Often, an image is based on another image, with some additional customization (i.e. build an image which is based on the ubuntu image, but installs the Apache web server).
    - To build an image, you create a **Dockerfile** with a simple syntax for defining the steps needed to create the image and run it. Each instruction in a Dockerfile creates a **layer in the image**. When you change the Dockerfile and rebuild the image, only those layers which have changed are rebuilt. This is part of what makes images so lightweight, small, and fast, when compared to other virtualization technologies.

## Docker Execution Process

The following happens When you run this command: `docker run -i -t ubuntu /bin/bash`:

1. Starts by **searching a local image** of `ubuntu`. If you do not have the ubuntu image locally, **Docker pulls** it from your configured registry, as though you had run `docker pull ubuntu` manually. So a local image is created (if it doesn't exist) and pointed at via the IMAGE ID reference 

2. Docker creates a **new container** with a new container ID based on the aforementioned image, as though you had run a docker container create command manually.

3. Docker allocates a **read-write filesystem** to the container, as its **final layer**. This allows a running container to create or modify files and directories in its local filesystem.

4. Docker creates a **network interface** to connect the container to the default network, since you did not specify any networking options. This includes assigning an IP address to the container. By default, containers can connect to external networks using the host machine’s network connection.

5. Docker **starts the container and executes /bin/bash**. Because the container is running interactively and attached to your terminal (due to the -i and -t flags), you can provide input using your keyboard while the output is logged to your terminal.

6. When you type **exit to terminate the /bin/bash command**, the container stops but is not removed. You can start it again or remove it.



## Basic Docker `-it` Option

- Without [Option] --> `docker run ubuntu`
    - Searches for Ubuntu image(copy from registry if none)
    - Creates a container from this image (a new container with a new ID)
    - **Immediately terminates** (as there is no such a container in `docker ps`) because there is no command (processes) after the creation of the container that required to be executed. 
    - Nevertheless this container was executed correctly and completely as there is such a container process in history: `docker ps -a`
    
- With [Option]= `-it` --> `docker run -it ubuntu`
    - Searches for Ubuntu image(copy from registry if none)
    - Creates a container from this image (a new container with a new ID)
    - You have a bash/shell access as a root user with system name the container ID.
    - this shell is not your server's shell, it is a fully isolated new system with a new shell access
    - if you open a new terminal and execute `docker ps` you will see this container running
    - you may exit this shell, as usual with `exit` command and then also from the `docker ps` with be terminated.
    


## Docker Compose
        


# References

- [Video - The Complete Practical Docker Guide](https://subscription.packtpub.com/video/cloud-networking/9781803247892)
- [Docker overview](https://docs.docker.com/get-started/overview)
- [Docker image vs container](https://circleci.com/blog/docker-image-vs-container/)


```{ - [Text](https:///.com) }
```

