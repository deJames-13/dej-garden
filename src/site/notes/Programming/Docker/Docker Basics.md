---
{"dg-publish":true,"permalink":"/programming/docker/docker-basics/","noteIcon":""}
---


> [!important] **Installation**
> 1. Go to  [Docker Installation](https://docs.docker.com/get-docker)
> 2. Install Docker Engine and Docker Desktop
> 3. For Arch Linux, enable docker using `sudo systemctl enable docker.service`

> [!INFO] **Docker Images**
> Go to [Docker Hub](https://hub.docker.com) to explore images used for containers.

---

> [!important] Dockerfile
> The base file on which docker engine references for building containers


#### Basic Commands

> [!info] **FROM**
> specifies the base image to use
> format: FROM image\[:tag\] \[AS name\]
> 
> #Example  
> ```docker
> FROM ubuntu:20.04
> ```


> [!NOTE] **WORKDIR**
> Starting path for the container
> ```docker
> # WORKDIR /path/to/directory
> #Example 
> WORKDIR /app


> [!NOTE] **COPY**
> Copies files and directories to build


> [!NOTE] **RUN**
> Executes shell commands


> [!NOTE] **EXPOSE**
> Tells docker to listen to specific ports at runtime


> [!NOTE] **ENV**
> Setting variable environments


> [!NOTE] **ARG**
> Defines build time variables


> [!NOTE] **Volume**
> Mount point of extra storage


> [!NOTE] **CMD**
> Provides default command to execute


> [!NOTE] **Entry Point**
> Default executable when the command start

