# Operations Engineering

## Toolkit for Software Management 

#

## User & Group Management - Syllabus 

- #### 1. [Users Management](#Users-Management)
    - ##### 1.1. [`/etc/passwd` File](#/etc/passwd-File)
    - ##### 1.2. [`/etc/passwd` Fields](#/etc/passwd-Fields)
    - ##### 1.3. [Add Users (Actual & System Users)](#Add-Users-(Actual-Users-&-System-Users))
    - ##### 1.4. [Remove Users (Actual & System Users)](#Remove-Users-(Actual-Users-&-System-Users))
    - ##### 1.5. [Set Password for Users](#Set-Password-for-Users)
    - ##### 1.6. [Modify Users' info](#Modify-Users'-info)
    - ##### 1.7. [`/etc/shadow` File](#/etc/shadow-File)
    - ##### 1.8. [`/etc/passwd` Fields](#/etc/passwd-Fields)

- #### 2. [Group Management](#Group-Management)
    - ##### 2.1. [`/etc/group` File](#/etc/group-File)
    - ##### 2.2. [`/etc/group` Fields](#/etc/group-Fields)
    - ##### 2.3. [Add Groups](#Add-Groups)
    - ##### 2.4. [Remove Groups](#Remove-Groups)
    - ##### 2.5 [Practical Groups Management Examples](#Practical-Groups-Management-Examples)
        - ##### 2.3.1 [SSH Group](#SSH-Group)

- #### 3. [Permissions](#Permissions)
    - ##### 3.1. [Setting Permissions](#Setting-Permissions)
    - ##### 3.2. [Setting Permissions with numerical method](#Setting-Permissions-with-numerical-method)
    - ##### 3.3. [Special Permissions](#Special-Permissions)
    - ##### 3.4. [Setting Special Permissions](#Setting-Special-Permissions)
    - ##### 3.5 [Setting Special Permissions with numerical method](#Setting-Special-Permissions-with-numerical-method)


## Docker Administration - Syllabus 

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