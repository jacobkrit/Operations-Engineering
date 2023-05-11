# Docker Administration 

All In One Docker Guide with the main concepts 

- #### 1. [Docker Linux Installation](#Docker-Linux-Installation)
- #### 2. [Docker Commands](#Docker-Commands)
    - ##### 2.1. [Basic Docker-Commands](#Basic-Docker-Commands)
    - ##### 2.2. [Alternative-Docker Commands](#Alternative-Docker-Commands)
    - ##### 2.3. [Basic Docker Options](#Basic-Docker-Options)
- #### 3. [Docker-Architecture](#Docker-Architecture)
    - ##### 3.1. [Concept of Docker](#Concept-of-Docker) 
    - ##### 3.2. [Docker Objects](#Docker-Objects) 
        - ##### 3.2.a [Container Object](#Container-Object)
        - ##### 3.2.b [Image Object](#Image-Object) 
- #### 4. [Image Creation Methods](#Image-Creation-Methods)
    - ##### 4.1. [Interactive Method](#(1)-Interactive-Method) 
        - ##### 4.1.a [Core Docker Execution Process](#Core-Docker-Execution-Process-(Interactive-Method)) 
        - ##### 4.1.b [Basic Docker `-it` Option](#Basic-Docker--it-Option-(Interactive-Method))
    - ##### 4.2. [Dockerfile Method](#(2)-Dockerfile-Method)      
- #### 5. [Dockerfile](#Dockerfile)
    - ##### 5.1. [Core Dockerfile Execution Process](#Core-Dockerfile-Execution-Process) 
    - ##### 5.2. [Dockerfile Commands](#Dockerfile-Commands) 
- #### 6. [Environment Variables](#Environment-Variables)
    - ##### 6.1. [Example Environment Variables](#Dockerfile-Example-Environment-Variables) 
- #### 7. [Docker Compose](#Docker-Compose)
    - ##### 7.1. [Docker Compose Execution Process](#Docker-Compose-Execution-Process)  
    - ##### 7.2. [Basic docker `compose.yml` Format](#Basic-docker-compose.yml-Format)    
    - ##### 7.3. [Docker Compose Services](#Docker-Compose-Services) 
        - ##### 7.3.1 [(Example) Pulling an Image](#Example-1---Pulling-an-Image) 
        - ##### 7.3.2 [(Example) Building One Simple Image](#Example-2---Building-One-Simple-Image) 
        - ##### 7.3.3 [(Example) Building One Advanced Image](#Example-3---Building-One-Advanced-Image) 
        - ##### 7.3.4 [(Example) Building Multiple Images (Different Directories)](#Example-4---Building-Multiple-Images-(Different-Directories)) 
        - ##### 7.3.5 [(Example) Building Multiple Images (Different Directories)](#Example-5---Building-Multiple-Images-(Different-Directories)) 
        - ##### 7.3.6 [(Example) Building Remote Images](#Example-6---Building-Remote-Images) 
    - ##### 7.4. [Docker Compose Network](#Docker-Compose-Network)  
        - ##### 7.4.1 [(Example)]()
    - ##### 7.5. [Docker Compose Environment Variables](#Docker-Compose-Environment-Variables)
        - ##### 7.5.1. [Environment Variables from a file (`env`file)](#Environment-Variables-from-a-file-(env_file))      
- #### 8. [References](#References-and-Appendix)
    
    
# 
## Docker Linux Installation
Installation steps for Server applications:
1. Follow the official installation guidelines ([Docker Linux Installation](https://docs.docker.com/engine/install/debian/)) 
2. Verify the successful installation: `sudo docker run hello-world`
3. Better practice is to execute docker as a **non-root user**. When docker is installed a docker group usually is created which grants root-level privileges to the users added. As we discussed at the `Linux-User-Group-Administration.md` we need:
    - check if a docker group exists: `cat /etc/group | grep docker` (if doesn't exist create one: `sudo groupadd docker`)
    - check the users which currently are members of the docker group: `groups docker` (if it's a new docker installation it should be empty)
    - add users to it: `sudo usermod -aG docker [USERNAME]` ([Manage Docker as a non-root user](https://docs.docker.com/engine/install/linux-postinstall/))
    - restart the server
    - verify the execution without sudo privileges: `docker run hello-world`

#
## Docker Commands

### Basic Docker Commands

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
|`docker rm [OPTIONS] [Container_Name]`| delete one or more containers|  
|`docker logs [Container_ID]`|helps debugging|  
|`docker exec -it Container_ID COMMAND`|run a command in a running container|  
|`docker exec -it Container_ID /bin/bash`|get the bash as vm as a root user|  
|`docker commit`|create a new image from the container image|  
|`docker push`|push an image or repository to a registry|  
|`docker network ls`|Available networks, used for intercommunication between containers|  
|`docker network create NETWORK_NAME`|create a docker network|  
|` `| |  

There are two categories of commands (basic commands & management commands) in management commands arguments (options) are required


<img src="images/docker-commands.jpg" alt="docker-commands" width="500px">

### Alternative Docker Commands

| Command  | Command| Description|
|---|---|---|
|`docker container ls`|`docker ps`| list of processes | 
|`docker builder build `|`docker build`| build a new image |
|`docker container inspect`|`docker inspect`| inspect specific container |
|`docker container commit`|`docker commit`| create image on specific container|
|`docker container run`|`docker run`| start a new container |
 
### Basic Docker Options

| [OPTIONS]   | Description|
|---|---| 
|`-d`| detached mode : run the container without the terminal: `docker run -d [IMAGE_NAME]`| 
|`-a`| Print container that are running and not running | 
|`- p[Host Port Number]:[Binding Port Number]`| Bind port of your host (port of the host <-- port which you exernaly send requests) to the container (port of tha you are binding this). **Port**: specifies on which port the container is listening to the incomming request. example: `docker run -p6000:6379 [IMAGE_NAME]`| 





#
## Docker Architecture

### Concept of Docker

Docker uses a Client/Server Architecture. The Docker Client (AKA **docker**) talks to the Docker Server/Host (AKA **daemon**):
- **Client/docker**: is the primary way that users interact with Docker. By using commands (example: `docker pull [OPTIONS] [IMAGE_NAME]`) the client sends these requests to Server/daemon, which carries them out. The Docker Client can communicate with more than one Server/daemon.
- **Server/daemon**: manages objects such as images, containers, networks, and volumes. Basically daemon does the heavy lifting of building, running, and distributing your Docker containers. A daemon can also communicate with other daemons to manage Docker services.
- Client and Server/daemon can run on the same system, or you can connect a Docker client to a remote Docker Server/daemon. 
- Client and Server/daemon communicate using a **REST API**, over UNIX sockets or a network interface.
- **Docker Compose** is a type of Client, that lets you work with applications consisting of a set of **containers**.
- **Registry**: stores Docker **images**. Docker Hub is a public registry, and Docker is configured to look for images on Docker Hub by default. By using commands such as `docker pull [OPTIONS] [IMAGE_NAME]` or `docker run [OPTIONS] [IMAGE_NAME]`, the required images are pulled from your configured registry. When you use the `docker push` command, your image is pushed to your configured registry.

<img src="images/docker-architecture.jpg" alt="docker-architecture" width="500px">

### Docker Objects

#### Container Object

- **Container**: is an **isolated** place where an **application** runs without affecting the rest of the system and without the system impacting the application. Because they are isolated, containers are well-suited for securely running software like databases or web applications that need access to sensitive resources without giving access to every user on the system.
    - Containers run natively on Linux and shares the host machine’s kernel, they are **lightweight**, not using more memory than other executables. If you stop a container, it will not automatically restart unless you configure it that way. Containers can be much **more efficient** than virtual machines because they don’t need the overhead of an entire operating system. They share a single kernel with other containers and boot in seconds instead of minutes.
    - Use containers for **packaging an application** with all the components it needs, then ship it all out as one unit. This approach is popular because it eliminates the friction between development, QA, and production environments, enabling faster software shipping. Building and deploying applications inside software containers eliminates “works on my machine” problems when collaborating on code with fellow developers.
    - You can **create**, **start**, **stop**, **move**, or **delete** a container using the **Docker API** or CLI. You can connect a container to one or more networks, attach storage to it, or even create a new image based on its current state.
    - **Container Layers** Each time Docker launches a container from an image, it adds a thin writable layer, known as the container layer, which stores all changes to the container throughout its runtime. As this layer is the only difference between a live operational container and the source Docker image itself, any number of like-for-like containers can potentially share access to the same underlying image while maintaining their own individual state.


#### Image Object

- **Image**: is a read-only template with instructions for creating a Docker container. Think of an image like a **blueprint** or snapshot of what will be in a container when it runs. Basically an image bundles together all the essentials such as installations, application code, and dependencies in order to execute code in a Docker container.
    - Docker **container as a running image instance**. You can create many containers from the same image, each with its own unique data and state.
    - Often, an image is based on another image, with some additional customization (i.e. build an image which is based on the ubuntu image, but installs the Apache web server).
    - To build an image, you create a **Dockerfile** with a simple syntax for defining the steps needed to create the image and run it. Each instruction in a Dockerfile creates a **layer in the image**. When you change the Dockerfile and rebuild the image, only those layers which have changed are rebuilt. This is part of what makes images so lightweight, small, and fast, when compared to other virtualization technologies.

    - **Image Layers** Each of the files that make up a Docker image is known as a layer. These layers form a series of intermediate images, built one on top of the other in stages, where **each layer is dependent on the layer immediately below it**. The **hierarchy of your layers is key to efficient** lifecycle management of your Docker images. Thus, you should organize layers that change most often as high up the stack as possible. This is because, when you make changes to a layer in your image, Docker not only rebuilds that particular layer, but all layers built from it.  
        - Change to a layer at the top of a stack involves the least amount of computational work to rebuild the entire image.
        - **Parent Image** is the first layer of a Docker image.  It’s the foundation upon which all other layers are built and provides the basic building blocks for your container environments.
            - A typical parent image may be a stripped-down Linux distribution or come with a preinstalled service
        - **Base image** is an empty first layer, which allows you to build your Docker images from scratch; give you full control over the contents of images.
    

#
## Image Creation Methods

You can create a Docker image by using one of **Two Methods**:

### (1) Interactive Method

- **Interactive**: By running a container from an existing Docker image (example run ubuntu: `docker run -it ubuntu`), manually changing that container environment through a series of live steps (example update: `apt-get update`), and saving the resulting state as a new image (find the container's id after the installation and from another terminal execute: `docker commit containers_id`) you will find this update container as a new image when you execute: `docker images`.
    - Advantages: Quickest and simplest way to create Docker images. Ideal for testing, troubleshooting, determining dependencies, and validating processes.
    - Disadvantages: Difficult lifecycle management, requiring error-prone manual reconfiguration of live interactive processes.  Create unoptimized images with unnecessary layers.
    
    
#### Core Docker Execution Process (Interactive Method)

The following happens when you run this command: `docker run -i -t ubuntu /bin/bash`:

1. Starts by **searching a local image** of `ubuntu`. If you do not have the ubuntu image locally, **Docker pulls** it from your configured registry, as though you had run `docker pull ubuntu` manually. So a local image is created (if it doesn't exist) and pointed at via the IMAGE ID reference 

2. Docker creates a **new container** with a new container ID based on the aforementioned image, as though you had run a docker container create command manually.

3. Docker allocates a **read-write filesystem** to the container, as its **final layer**. This allows a running container to create or modify files and directories in its local filesystem.

4. Docker creates a **network interface** to connect the container to the default network, since you did not specify any networking options. This includes assigning an IP address to the container. By default, containers can connect to external networks using the host machine’s network connection.

5. Docker **starts the container and executes /bin/bash**. Because the container is running interactively and attached to your terminal (due to the -i and -t flags), you can provide input using your keyboard while the output is logged to your terminal.

6. When you type **exit to terminate the /bin/bash command**, the container stops but is not removed. You can start it again or remove it.


#### Basic Docker `-it` Option (Interactive Method)

The `-it` option is the same as `-i -t` just merged. And has the following purpose:

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
    

### (2) Dockerfile Method

- **Dockerfile**: By constructing a plain-text file, known as a Dockerfile, which provides the specifications for creating a Docker image; **like a blueprint**. This is a three-step process whereby you create the Dockerfile and add the commands you need to assemble the image.
    - Advantages: Clean, compact and repeatable recipe-based images. Easier lifecycle management and easier integration into continuous integration (CI) and continuous delivery (CD) processes. Clear self-documented record of steps taken to assemble the image.
    - Disadvantages: More difficult for beginners and more time consuming to create from scratch.
    
    






#
## Dockerfile

Docker can **build images** automatically by reading the instructions from a Dockerfile. A Dockerfile is a text document that contains all the commands a user could call on the command line to assemble an image. Dockerfile is the blueprint of your project. 


### Core `Dockerfile` Execution Process

1. **Project Directory Creation**
    1. Make a folder with the files used in this Docker image.
    2. `cd` to this Directory.


2. **Create a Dockerfile**
    1. Create a file named `DockerFile`.
    2. Include the following lines in the `DockerFile` file:   

        `FROM ubuntu`   
        `WORKDIR /home/admin/docker_projects/[DIRECTORY_NAME]`    
        `RUN apt-get update`    
        `ENTRYPOINT pwd`    


3. **Build an image** based on this dockerfile:

    After saving the `DockerFile` file and while you are inside this directory execute:  
    
    ```docker build -t [MY_IMAGE_NAME]:[VERSION_NUMBER] .```


4. **Run the image** as a container:

    `docker run [MY_IMAGE_NAME]:[VERSION_NUMBER]`



### `Dockerfile` Commands

The following table shows you those Dockerfile statements you’re most likely to use:

| Command   | Purpose - Description|
|---|---| 
| `FROM <image>:<tag>` | (*Must included*) Specify the parent image| 
| `MAINTAINER <name>` | (*Optional*) set the Author field of the generated images| 
| `WORKDIR </path/to/workdir>` | (*Best Practice*) Set working directory for any commands that follow in the Dockerfile \* | 
| `ARG <key> <value>` | (*Optional*) Pass Docker Environment Variables During The Image Build  with `--build-arg`| 
|`RUN <command>` | (*Must included*) To install any applications and packages required for your container. \* | 
|`COPY <src> <dest>` |To copy over files or directories from a specific location. | 
|`ADD <src> <dest>` |As COPY, but also able to handle remote URLs and unpack compressed files.| 
| `ENV <key> <value>` | (*Optional*) Pass Docker Environment Variables afterwards once the container runs with `--env`| 
|`ENTRYPOINT <command> <param1> <param2>` | Command that will always be executed when the container starts. If not specified, the default is /bin/sh -c| 
| `CMD <command> <param1> <param2>` | Arguments passed to the entrypoint. If ENTRYPOINT is not set (defaults to /bin/sh -c), the CMD will be the commands the container executes. | 
|`EXPOSE <port>` | To define which port through which to access your container application.| 
|`LABEL <key>=<value>` |To add metadata to the image| 
| | | 

\* You can have multiple WORKDIR instructions in your Dockerfile which are build as the layers go down

\* the RUN statements in your Dockerfile are only executed during the build stage, i.e. using docker build and not with docker run


#
## Environment Variables

Docker ENV and ARG are pretty similar, but not quite the same.
- ARG can be set during the image build with `--build-arg` 
- ENV can be set during the container running with `--env`


### Dockerfile Example Environment Variables

1. **Project Directory Creation**
 
    1. Make a folder with the files used in this Docker image
    2. `cd` to this Directory


2. **Create a Dockerfile**

    1. Create a file named `DockerFile`
    2. Include the following lines in the `DockerFile` file: 

        `FROM ubuntu`  
        `WORKDIR /home/admin/docker_projects/proj2`  
            
        `ARG my_arg`  
        `RUN echo $my_arg`  
    
        `ENV my_var=$my_arg`
        `ENTRYPOINT echo $my_var`


3. **Build an image** based on this dockerfile:

    After saving the `DockerFile` file and while you are inside this directory execute:

    `docker build -t my_proj2 --build-arg my_arg="TEST_VAR" .`
    
    As you will see during the build the value TEST_VAR will be printed

4. **Run the image** as a container:

    1. **Without** Environment Variable

        ` docker run my_proj2`
    
     As you will see the value **TEST_VAR** will be printed
    
     2. **With** Environment Variable

        ` docker run --env my_var='TEST_VAR2' my_proj2`
    
     As you will see the value **TEST_VAR2** will be printed


#
## Docker Compose

Docker Compose offers a convenient solution for streamlining the process of **building multiple images and running containers**: 

- Rather than manually creating each image and running containers one by one from the command line, Docker Compose allows you to perform these tasks all at once, by just including a `docker-compose.yml` file and executing `docker-compose [OPTIONS]` command.

- Docker Compose is similar to a **Linux .sh** file that contain multiple Linux commands, which can be executed all at once.

- Docker Compose **builds upon the concept of the DockerFile**, which serves as the blueprint for creating a single Docker image and running a container based on that image. **Expanding on this idea**, Docker Compose provides a **higher-level blueprint** for building multiple single Docker images and creating multiple containers with different dependencies.

- Just like the DockerFile, Docker Compose utilizes a `docker-compose.yml` file. This file acts as a guide for building images and running containers with their respective dependencies based on the images. It's important to note that the `docker-compose.yml` file **does not overwrite the DockerFile for each image; instead, it works in conjunction with it by providing instructions on how each individual DockerFile should be built**.


### Docker Compose Execution Process

Using Compose is essentially a **three-step process**:

1. For each independent apps/docker-images you want to build, as usual, create a folder with a `Dockerfile` file inside, as mentioned above (no difference).
2. Create one file (the docker compose directory) with `docker-compose.yml` which will supervise all the independent apps/docker-images and their containers
3. Run a `docker-compose up` command and the Docker compose command starts and runs your entire app. Execute the command (no containers should be running): `docker-compose [OPTIONS] up`. 


### Basic `docker-compose.yml` Format

In the `docker-compose.yml` file, we need to specify the version of the Compose file, at least one **service** (an image), and optionally volumes and networks:
- **services** refer to the containers' configuration based on images, each service may be independent of each other.
- **volumes** are physical areas of disk space shared between the host and a container, or even between containers. In other words, a volume is a shared directory in the host, visible from some or all containers.
- **networks** define the communication rules between containers, and between a container and the host. Common network zones will make the containers' services discoverable by each other, while private zones will segregate them in virtual sandboxes.

```
version: "3.7"
services:
    service-1:
        image: service-1-image
        ...
    service-2:
        image: service-2-image
        ...
    service-3:
        image: service-3-image
        ...
    ...
volumes:
    ...
networks:
    ...
```

### Docker Compose Services

There are multiple settings that we can apply to services the core ones are as follows, AND we can **combine** the followings:

#####
#### Example 1 - Pulling an Image
- In the `docker-compose.yml` you may use already builded images and run container based on them. These images may be custom images available locally at `docker images` or in the Docker Registry: 
    ```
    services: 
        ...
        service-X:
            image: ubuntu:latest
            container_name: my-ubuntu-container
        ...
        service-Y:
            image: my_custom_image:latest
            container_name: my-custom-container
        ...
    ```
- The `container_name` refers to the name when this container will be running
    
#####
#### Example 2 - Building One Simple Image
- In the `docker-compose.yml` you may build an image available in the same repository. You need to have a `Dockerfile` under the same path with the usual instructions.

    ```
    services: 
        ...
        service-X:
            build: .
            image: my-service-X-image-name
            container_name: my-service-X-container
        ...
    ```    
- The `image` refers to the name when this image will be build
- The `container_name` refers to the name when this container will be running

#####
#### Example 3 - Building One Advanced Image
- More formally you may use the  specify the build under `context` and optionally use Dockerfile Specific file and arguments:    
    ```
    services: 
        ...
        service-X:
            build:
                context: .
                dockerfile: Dockerfile-Specific-name
                args:
                    ARG_NAME_1: ARG_VAL_1
                    ARG_NAME_2: ARG_VAL_2
                    ...
            image: my-service-X-image-name
            container_name: my-service-X-container
        ...
    ```    
- The `context` refers to the path where the DockerfileSpecific-name exists. if `.` is the same path 
- The `dockerfile` refers to the particular Dockerfile Specific name.
- The `args` refers to the  arguments passed during the image build as discussed above
- The `image` here refers to the name when this image will be build
- The `container_name` here refers to the name when this container will be running

#####
#### Example 4 - Building Multiple Images (Different Directories)
- In the `docker-compose.yml` you may build multiple images available in same repository. You need to have a `Dockerfile-X` with the usual instructions at each of the repositories.    
    ```
    services: 
        ...
        service-X:
            build: .
            dockerfile: Dockerfile-X
            build: /path/to/image-service-X/dockerfile/
            image: my-service-X-image-name
            container_name: my-service-X-container
        ...
        service-Y:
            build: .
            dockerfile: Dockerfile-Y
            image: my-service-Y-image-name
            container_name: my-service-Y-container
        ...
    ```   

- This `dockerfile:` defines from which `Dockerfile` each is should be build
- This `path/to/dockerfile` for each image may be absolute or relevant.
- The `image` here refers to the name when this image will be build.
- The `container_name` here refers to the name when this container will be running

    

#####
#### Example 5 - Building Multiple Images (Different Directories)
- In the `docker-compose.yml` you may build multiple images available in the different repositories (app-image repositories). You need to have a `Dockerfile` with the usual instructions at each of the repositories.    
    ```
    services: 
        ...
        service-X:
            build: /path/to/image-service-X/dockerfile/
            image: my-service-X-image-name
            container_name: my-service-X-container
        ...
        service-Y:
            build: /path/to/image-service-Y/dockerfile/
            image: my-service-Y-image-name
            container_name: my-service-Y-container
        ...
    ```   

- This `path/to/dockerfile` for each image may be absolute or relevant.
- The `image` here refers to the name when this image will be build.
- The `container_name` here refers to the name when this container will be running


#####
#### Example 6 - Building Remote Images
- In the `docker-compose.yml` you may build multiple images available in the different repositories (app-image repositories). You need to have a `Dockerfile` with the usual instructions at each of the repositories.    
    ```
    services: 
        ...
        service-X:
            build: https://github.com/my-company/my-project.git
            image: my-service-X-image-name
        ...
    ```   
- The `image` here refers to the name when this image will be build
- The `container_name` here refers to the name when this container will be running

       
       
### Docker Compose Network

The multi-containers we run via Docker Compose have the option to communicate between themselves. 

#####
#### Example 1 - Simple Network
- A service can communicate with another service on the same network by simply referencing it by the container name and port (for example network-example-service:80), provided that we've made the port accessible through the expose keyword
```
services:
    network-example-service:
        image: karthequian/helloworld:latest
        expose:
            - "80"
```

#####
#### Example 2 - Multi Ports
- To reach a container from the host, the ports must be exposed declaratively through the ports keyword, which also allows us to choose if we're exposing the port differently in the host:
```
services:
    network-example-service:
        image: karthequian/helloworld:latest
        ports:
            - "80:80"
        ...
    my-custom-app:
        image: myapp:latest
        ports:
            - "8080:3000"
        ...
    my-custom-app-replica:
        image: myapp:latest
        ports:
            - "8081:3000"
        ...
```
- Port 80 will now be visible from the host, while port 3000 of the other two containers will be available on ports 8080 and 8081 in the host. This powerful mechanism allows us to run different containers exposing the same ports without collisions.


### Docker Compose Environment Variables
    
There are three types of volumes: anonymous, named, and host.

# +++
    
### Environment Variables from a file (env_file)

As not to insert Environment Variables on the fly it's best practice to put values into file. Those are used with Docker Compose and Docker Stack. It has nothing to do with ENV, ARG, or anything Docker-specific explained above. It’s exclusively a docker-compose.yml thing.

# +++
  
```{ 
version: '3'
services:
    mongodb:
        image: mongo
        ports:
            - 27017:27017
        environment:
            - MONGO_INITDB_ROOT_USERNAME=admin
            - MONGO_INITDB_ROOT_PASSWORD=password
        mongo-express:
            image: mongo-express
        ports:
            - 8080:8081 
        environment:
            - ME+CONFIG MONGODB_ADMINUSERNAME=admin
            - ME_CONFIG_MONGODB_ADMINPASSWORD=password
            - ME_CONFIG_MONGODB_SERVER=mongodb
}
```





# References and Appendix

- #### [Video - The Complete Practical Docker Guide](https://subscription.packtpub.com/video/cloud-networking/9781803247892)
- #### [Docker overview](https://docs.docker.com/get-started/overview)
- #### [Docker image vs container](https://circleci.com/blog/docker-image-vs-container/)
- #### [Understanding and Building Docker Images](https://jfrog.com/knowledge-base/a-beginners-guide-to-understanding-and-building-docker-images/)
- #### [Dockerfile](https://kapeli.com/cheat_sheets/Dockerfile.docset/Contents/Resources/Documents/index)
- #### [Pass Docker Environment Variables During The Image Build](https://vsupalov.com/docker-build-pass-environment-variables/)
- #### [Docker Compose will BLOW your MIND -Video](https://www.youtube.com/watch?v=DM65_JyGxCo)
- #### [Key features and use cases](https://docs.docker.com/compose/features-uses/)
- #### [Docker Compose](https://www.baeldung.com/ops/docker-compose)

```{ - [Text](https:///.com) }
```

