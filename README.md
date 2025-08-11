# 🐳 Containerization Tutorial

This tutorial will walk you through creating, running, and exploring a Docker container. Additionally, there are steps below for doing the same with Singularity/Apptainer.

---
# What is a Container?

Imagine you have just trained a deep learning model, but your collaborators across the country can’t get it to run. They keep running into missing library errors, mismatched package versions, or hardware-specific quirks. You spend hours troubleshooting over Zoom, only to realize their system setup is completely different from yours.  

This is the problem containers solve. By packaging your code, dependencies, and environment into one portable unit, you can send your work to anyone, anywhere, and be confident it will run exactly the same way it does on your own machine. No more “it works on my computer” headaches — containers bring reproducibility, portability, and peace of mind to collaborative science.

A **container** is a lightweight, standalone package that includes everything needed to run an application — code, dependencies, libraries, and configuration — all bundled together. Containers ensure that your application will run the same way on any system, whether it's your laptop, a server, or in the cloud. They are like “portable environments” that isolate your software from the host system, making development, testing, and deployment more consistent and reliable.

By following this tutorial, you will learn not only how to build a Container, but also how to run, debug, and share your containers with others.

---

## 0️⃣ Install Docker Desktop

The first step is to actually install software necessary for running and building these containers. There are many different containerization platforms that exist, but one of the most widely-used is **Docker**. Additionally, Docker is available on multiple operating systems, making it a good way to get your feet wet by using your own personal computer.

While you can use windows directly (if you have a windows computer), I would recommend instead installing WSL so that you can run Linux on your windows computer: [Install WSL](https://learn.microsoft.com/en-us/windows/wsl/install)

If you don’t already have Docker installed:  
- **Windows / macOS:** [Download Docker Desktop](https://www.docker.com/products/docker-desktop) and follow the setup instructions.  
- **Linux:** [Install Docker Engine](https://docs.docker.com/engine/install/) for your distribution.

To verify your installation, open a terminal and run the following command:
```bash
docker --version
```

---

## 1️⃣ Clone this repository

To have a template to work from, you can clone this github repository on your personal computer:

```bash
git clone https://github.com/<your-username>/AI_Summer_School_2025_Containerization.git
cd AI_Summer_School_2025_Containerization
```

---

Before we create a container, we need to understand **how** one is built in the first place. At the heart of every container is a **Dockerfile** — a simple text file containing a list of instructions that tells Docker exactly how to assemble the container image. These instructions specify things like which base image to start from (e.g., Ubuntu, Python, or a specialized deep learning image), which software packages to install, which files to copy in, and what commands to run when the container starts.

When you build a Docker image, Docker pulls the specified base image from a remote repository (usually Docker Hub), then **layers** your custom instructions on top of it. This layered approach makes builds efficient and reusable — you can start from a known, stable foundation and add only what your application needs.

---

## 2️⃣ Build the Docker image

After cloning the repo, you should have a Dockerfile to build your first Docker image from. However, we must first start the Docker daemon before creating or running images

```bash
cd Docker
docker build -t my_docker .
```
- `-t my-tutorial-image` gives the image a name.
- `.` tells Docker to use the Dockerfile in the current directory.


This should only take a little while, and you can watch the container build in real time. 

---




## 3️⃣ Run the Docker container
```bash
docker run --rm my_docker
```

---


## 4️⃣ Shell into the Docker container
```bash
docker run --rm -it my_docker /bin/bash
```

- Singularity/Apptainer as well
- Docker to singularity for HPC
- 








