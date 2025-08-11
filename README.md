# 🐳 Containerization Tutorial

This tutorial will walk you through creating, running, and exploring a Docker container. Additionally, there are steps below for doing the same with Singularity/Apptainer.

- [What is a Container?](#what-is-a-container)
- [0️⃣ Install Docker Desktop](#0-install-docker-desktop)
- [1️⃣ Clone this repository](#1-clone-this-repository)
- [2️⃣ Build the Docker image](#2-build-the-docker-image)
- [3️⃣ Run the Docker container](#3-run-the-docker-container)
- [4️⃣ Shell into the Docker container](#4-shell-into-the-docker-container)
- [5️⃣ Push Your Docker Image to Docker Hub](#5-push-your-docker-image-to-docker-hub)
- [6️⃣ Pull a Container From Docker Hub](#6-pull-a-container-from-docker-hub)
- [BONUS: Singularity/Apptainer](#6-Singularity/Apptainer)
- [BONUS: Convert Docker to Singularity](#bonus-convert-docker-to-singularity)

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

You may be required to create a Docker account. To facilitate later steps, I suggest creating one at the time of installation.

To verify your installation, open a terminal and run the following command:
```bash
docker --version
```

---

## 1️⃣ Clone this repository

To have a template to work from, you can clone this github repository on your personal computer:

```bash
git clone https://github.com/mkim-MASI/AI_Summer_School_2025_Containerization.git
cd AI_Summer_School_2025_Containerization
```

---

Before we create a container, we need to understand **how** one is built in the first place. At the heart of every container is a **Dockerfile** — a simple text file containing a list of instructions that tells Docker exactly how to assemble the container image. These instructions specify things like which base image to start from (e.g., Ubuntu, Python, or a specialized deep learning image), which software packages to install, which files to copy in, and what commands to run when the container starts.

When you build a Docker image, Docker pulls the specified base image from a remote repository (usually Docker Hub), then **layers** your custom instructions on top of it. This layered approach makes builds efficient and reusable — you can start from a known, stable foundation and add only what your application needs.

---

## 2️⃣ Build the Docker image

After cloning the repo, you should have a Dockerfile to build your first Docker image from. However, we must first start the Docker daemon before creating or running images

#### On Windows and macOS:
Start Docker Desktop from your Applications or Start Menu. Wait until the Docker whale icon appears in your system tray or menu bar, indicating the daemon is running.

#### On Linux:
Start the Docker service with the command:

```bash
sudo systemctl start docker
```

You can check if it’s running with:
```bash
sudo systemctl status docker
```

Once the Docker daemon is running, you can build your image:
```bash
cd Docker
docker build -t my_docker .
```
- `-t my-tutorial-image` gives the image a name.
- `.` tells Docker to use the Dockerfile in the current directory.


This should only take a little while, and you can watch the container build in real time. Once the container is built, we can check to see that it exists by running:

```bash
docker images
```

If it had built successfully, should see something like:
```bash
REPOSITORY   TAG       IMAGE ID       CREATED        SIZE
my_docker    latest    892a562a7df4   11 hours ago   2.59GB
```

---

## 3️⃣ Run the Docker container

Now that we have built our container, we can run it! If you take a look at the Dockerfile we used to build our container, you can see that we installed many dependencies and python libraries. At the end, you will see the following line:

```bash
CMD ["python3", "/workspace/run_python_test.py"]
```

This line is telling the Docker what the default command that gets executed is for when we actually **run** the Docker. If you take a look at the `run_python_test.py` file that we copied into the Docker, you will see that it just prints out the version of Pytorch that is installed. (Very simple!)

To execute this command in our Docker container, run:
```bash
docker run --rm my_docker
```

You should see the output:
```bash
2.8.0+cpu
```

When we ran the command above, we had the flag `--rm`, which just says to stop the container after it has finished executing. For some instances, we would like our container to stay open in the background, but for this purpose, we can just close it after it has finished. There are additional flags that are rather helpful when running Docker containers. For example:

`-v` — Mounts (binds) a directory from your host machine into the container. This is essential for persisting data or sharing files between host and container, as Docker containers by default cannot see your entire filesystem (only what it needs or what you tell it!)
Example:
```bash
docker run -v /path/on/host:/path/in/container my-tutorial-image
```
In this example:

- `/path/on/host` is a directory on your machine
- `/path/in/container` is where it will appear inside the container
Any changes inside that path will be reflected both in your host and container.

---

## 4️⃣ Shell into the Docker container

So now we can run a container, but we also want to be able to understand how to look inside the container in case we need to debug issues. This is called **shelling** into the container, which will effectively allow us to naviagte through the Docker container as if we were navigating through our computer through a terminal window. To do so, run:
```bash
docker run --rm -it my_docker /bin/bash
```
- `-it` is a flag that runs the container in interactive mode (`-i`) with a terminal (`-t`), so you can interact with it directly.
- `/bin/bash` is where the `bash` shell is installed in the Docker (almost always here by default) for executing terminal commands.

Once inside the container, we can view files or details about the Docker. For example, we can check to see the operating system using:
```bash
cat /etc/os-release
```

You should see:
```bash
PRETTY_NAME="Debian GNU/Linux 12 (bookworm)"
NAME="Debian GNU/Linux"
VERSION_ID="12"
VERSION="12 (bookworm)"
VERSION_CODENAME=bookworm
ID=debian
HOME_URL="https://www.debian.org/"
SUPPORT_URL="https://www.debian.org/support"
BUG_REPORT_URL="https://bugs.debian.org/"
```

Which is cool! This is proof that we have a different operating system than our host computer (unless your host is already running Debian Bookworm)

To look at the python file, we can run
```bash
cat run_python_test.py
```

---

## 5️⃣ Push Your Docker Image to Docker Hub
Once you’ve built and tested your Docker image locally, you might want to share it with others or use it on different machines. Docker Hub is a popular online registry where you can push (upload) your images and pull (download) them later.

### Step 1: Create a Docker Hub account (if you haven't already)
Go to [Docker Hub](https://hub.docker.com/) and sign up for a free account.

### Step 2: Log in to Docker Hub from your command line
Run the following command and enter your Docker Hub username and password when prompted:
```bash
docker login
```

You should see:
```bash
Login Succeeded
```

### Step 3: Tag your local image for Docker Hub
Docker images need to be tagged with your Docker Hub username and repository name before pushing. The format is:
```bash
<dockerhub-username>/<repository-name>:<tag>
```

For example, if your Docker Hub username is `mkim` and you want to name your repo `my-docker-image` with the tag `latest`:
```bash
docker tag my_docker mkim/my-docker-image:latest
```

Note that a **tag** is a way to version control your container in case you make updates. This is especially helpful for continuous development so that users know which **exact** version of your code they are using.

### Step 4: Push the image to Docker Hub
Now, push your tagged image to Docker Hub:
```bash
docker push <dockerhub-username>/<repository-name>:<tag>

## example
docker push mkim/my-docker-image:latest
```
To verify that you have successfully pushed your container, you can visit:

https://hub.docker.com/repositories/<dockerhub-username>

where you should see your container! By default, when you create a new repository on Docker Hub, it might be set to private depending on your account type. Thus, if you want to share it with others, you need to make it public. To make it public:

- 1.) Go to https://hub.docker.com/ and log in.

- 2.) Click on your username (top right), then Repositories.

- 3.) Select the repository you want to make public.

- 4.) Go to the Settings tab.

- 5.) Scroll down to Repository Visibility.

- 6.) Choose Public and save the changes.

Now, anyone can pull your container from DockerHub!

### Alternative: Export Docker image as a file
Sometimes, you may want to share your Docker image without using an online registry like Docker Hub. For example, if you’re working on a secure network without internet access or want to transfer the image via USB. Docker lets you save your image to a file and load it on another machine.

#### Step 1: Save your Docker image to a tar file

Use the `docker save` command to export your image into a single tarball file:

```bash
# This command will create a file named my_docker_image.tar in your current directory.
docker save -o my_docker_image.tar my_docker
```
- `-o my_docker_image.tar` specifies the output filename.

You can then transfer this to someone else just as you would any other file.

#### Step 2: Load the .tar file

Once you have a Docker image as a `.tar` file, you can load it by running:
```bash
docker load -i my_docker_image.tar
```

Make sure that when you push your Docker, you provide plenty of documentation for how someone would need to run your container! Some helpful things to include:
- example input/output data
- example commands
- expected runtime / memory usage

---
## 6️⃣ Pull a Container From Docker Hub

Once you have pushed your Docker image to Docker Hub or want to try out someone else’s image, you can pull it from the command line and run it locally.

---

### Step 1: View your own image on Docker Hub from the command line

You can search Docker Hub for your images using the `docker search` command. However, this shows public images across all users.

To find your own images more reliably, you can visit your Docker Hub profile on the web.

Example search command:

```bash
docker search <your-dockerhub-username>
```

### Step 2: View someone else’s image
If your collaborator shares their Docker Hub repository name (for example, `collabuser/their-image`), you can also search or pull their image directly.
```bash
docker search collabuser/their-image
```

### Step 3: Pull the container (image) from Docker Hub
To download the image from Docker Hub to your local machine, run:
```bash
docker pull <dockerhub-username>/<repository-name>:<tag>
```

### Step 4: Check that the image is downloaded
After pulling, confirm that the Docker container is there:
```bash
docker images
```

And just like the container you built earlier, you can run/debug this one too!

---

At this point, you should have all the general knowledge needed for how to 1.) build a Docker container, 2.) run and debug your container, and 3.) share your container with others.

---

# BONUS: Singularity/Apptainer

Unfortunately, Docker requires super user (`sudo`) permissions to build/run containers. This can be problematic when you need to run code on high performance clusters (HPC) for large-throughput. To circumvent this issue, a containerization platform called **Singularity** (now called **Apptainer**) was created specifically for scientific computing purposes. The general framework is largely similar, with some syntax differences and other nuances.

---

## Install Singularity/Apptainer

- On most Linux systems, you can install Singularity using your package manager or from source.  
- For detailed instructions, visit: [https://apptainer.org/docs/admin/installation](https://apptainer.org/docs/admin/installation)

I would recommend installing an existing pre-built version from here: [Apptainer Releases](https://github.com/apptainer/apptainer/releases)

---

## Building a Singularity Container

```bash
cd ../Singularity
bash build_simg.sh
```

---

## Running a Singularity Container

One noteable difference is that Singularity/Apptainer does NOT explicity "isolate" the container from the rest of the host system by default. To do so, you MUST use the `-ec` flags. Thus, to run the container:
```bash
singularity run -ec myimg.simg
```

Note that just as you could bind directories using `-v` with Docker containers, you can similarly bind directories using `-B` with singularity containers:

```bash
singularity run -ec -B /path/to/outside/directory:/path/to/directory/in/container myimg.simg
```
---

## Shelling into a Singularity Container

```bash
singularity shell -ec myimg.simg
```
---

One of the nice things about Singularity/Apptainer containers is that they are just files, and thus can easily be transferred. Although there is no "Apptainer Hub," you can upload your containers to a backed-up, online repository like Zenodo: [Zenodo](https://zenodo.org/)

---

# BONUS: Convert Docker to Singularity

You can use the `singularity pull` command to convert a Docker image to a Singularity image (`.sif` file):

```bash
singularity pull docker://<dockerhub-username>/<repository-name>:<tag>
```






