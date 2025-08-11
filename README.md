# AI_Summer_School_2025_Containerization

🐳 Containerization Tutorial
This tutorial will walk you through creating, running, and exploring a Docker container.

0️⃣ Install Docker Desktop
If you don’t already have Docker installed:

Windows / macOS: Download Docker Desktop and follow the setup instructions.

Linux: Install Docker Engine for your distribution.

To verify your installation:

bash
Copy
Edit
docker --version
1️⃣ Clone this repository
bash
Copy
Edit
git clone https://github.com/<your-username>/<your-repo-name>.git
cd <your-repo-name>
2️⃣ Build the Docker image
bash
Copy
Edit
docker build -t my-tutorial-image .
-t my-tutorial-image gives the image a name.

. tells Docker to use the Dockerfile in the current directory.

3️⃣ Run the Docker container
bash
Copy
Edit
docker run --name my-tutorial-container my-tutorial-image
--name my-tutorial-container names your container.

This command runs the container based on the image we just built.

4️⃣ Shell into the Docker container
You can open an interactive shell inside the container:

bash
Copy
Edit
docker exec -it my-tutorial-container /bin/bash
From here, try:

bash
Copy
Edit
ls
You should see the files that were copied into the container from the repository.

🎯 What you learned
How to install Docker

How to clone a project with a Dockerfile

How to build a Docker image

How to run and explore a container

