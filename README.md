# Introduction to Docker and what a DevOps and Cloud engineer should know about this tool.

## Repo to learn Docker with examples. Contributions are most welcome.

## Requirements for Docker Desktop installation

**Docker Desktop on Windows**

***System Requirements:***

- Windows 10 64-bit (Pro, Enterprise, or Education, Build 19044 or newer)
- Windows 11 64-bit (all editions)
- WSL 2 (Windows Subsystem for Linux v2) must be installed and enabled
- Virtualization enabled in BIOS (Intel VT-x or AMD-V)
- At least 4 GB RAM (8 GB or more recommended)
- Hyper-V and Containers features are enabled in Windows

**Docker Desktop on macOS**  

***System Requirements:***

- macOS Monterey 12.5 or later, including Ventura
- Compatible with both Apple Silicon (M1/M2, Rosetta 2 needed for some x86 images) and Intel Macs
- Minimum 4 GB RAM

**Docker Desktop on Linux**  

***System Requirements (Preview/Stable for Some Distros):***

- Supported distributions: Ubuntu 22.04+, Debian 11+, Fedora 35+
- 64-bit architecture
- systemd as the init system
- Virtualization support
- At least 4 GB RAM
- Kernel version 5.15 or newer

### General Requirements for All Platforms:

- Administrative or root access for installation
- Reliable internet connection to download Docker images and updates

## Commonly used terminology: Docker Engine, Docker Daemon, Docker Desktop, Dockerhub, Docker Registry

***1. Docker Engine***

- The main part of Docker, responsible for building, running, and managing containers.  

**It works as a client-server application and includes:**

- Docker Daemon (`dockerd`)
- Docker Command Line Interface (docker)
It operates on Linux and handles all container-related tasks.

***2. Docker Daemon (`dockerd`)***

- This is the background process that manages containers.  
- It listens for Docker API requests (from the CLI, Docker Desktop, etc.), and is responsible for starting and
  stopping containers, as well as managing images, volumes, and networks.  
- It’s a key part of the Docker Engine.

***3. Docker Desktop*** 

- An all-in-one application for Mac and Windows that provides a graphical interface and environment for Docker.
  
**It comes with:**
  
- Docker Engine (including the Daemon)
- Docker CLI
- Optional Kubernetes support
- A GUI dashboard
- WSL 2 integration (on Windows)

- Docker Desktop simplifies Docker usage for developers by handling setup and configuration.
  
***4. Dockerhub***

**Docker Hub**  

- An online registry service (similar to GitHub, but for Docker images).
- It allows you to upload, download, and share Docker images.
- Supports both public and private repositories.

## Breakdown note:

| Component      | Role                                                   |
| -------------- | ------------------------------------------------------ |
| Docker Desktop | GUI + bundled setup for developers                     |
| Docker Engine  | The container platform powering everything             |
| Docker Daemon  | The backend service that does the work (inside Engine) |
| Docker Hub     | Cloud-based image storage & distribution service       |
 
# Docker Architecture  

![image](https://github.com/user-attachments/assets/6026d7f1-9c85-4bdc-a173-2cbd1345c3fd)

## Link to setup your Dockerhub account:

https://hub.docker.com/

# Prerequisites

## 1. Basic Command Line Skills

***You should be comfortable with:***

* Navigating directories (cd, ls, pwd)

* Editing files (nano, vim, or using VS Code)

* Running scripts and installing packages

## 2. Operating Systems & Virtualization Concepts

* Understand how traditional applications run on OSes

* Know what virtual machines are (optional but useful)

* Learn the difference between VMs vs Containers

## 3. Networking Basics

* What are ports and IP addresses

* What is localhost

* Basics of client-server communication (e.g., HTTP)

## 4. Software Development & Deployment Basics

* Know what builds, environments, and dependencies are

* Familiarity with package managers (e.g., npm, pip, apt)

* Concept of an application stack (e.g., Node.js + MongoDB)

## 5. Understanding of Source Control (Git)

* You don’t need to be a Git expert, but you should:

* Know how to clone, commit, and push

* Be able to manage basic branches

## 6. Familiarity with YAML & Configuration Files

* Docker and related tools (like Docker Compose or Kubernetes) rely heavily on configuration in YAML and .env files.

## 7. Optional but Helpful: Linux Fundamentals

* Knowing basic file system layout (e.g., /etc, /usr, /var)

* Managing permissions

* Running services or daemons

* Once you're comfortable with these, start learning Docker with small projects—like containerizing a simple web app.

  

# 🐳 Getting Started with Docker: 

## 1. What is Docker?

* Docker is a containerization tool that packages applications and their dependencies into containers.
  Containers are lightweight, fast, and portable across environments—ideal for DevOps.

## 2. Why Use Docker?

* Works the same on any system (dev, staging, prod)

* Fast startup, isolated environments

* Easy to scale and integrate with cloud and CI/CD

## 3. Install Docker

**Ways to install Docker: Cloud-based installation or locally based installation. For the latter,**
**Find the link below. Window users, please make sure to check if your Windows OS is AMD64 or ARM64**
**before you start any installation.**

* Windows/macOS: Docker Desktop Installation

***Docker Desktop installation***

```
https://www.docker.com/products/docker-desktop
```

**Docker Desktop System Requirements, etc.**

```
https://docs.docker.com/desktop/setup/install/windows-install/
```
  
***Create a DockerHub account:***

```
https://app.docker.com/signup/?_gl=1*1az2fao*_ga*NjgzOTMzNjQzLjE3NDkyMDgyMDk.*_ga_XJWPQMJYHQ*czE3NDkyMDgyMDgkbzEkZzEkdDE3NDkyMDgyMDkkajU5JGwwJGgw
```


* Check your Windows OS: Open Settings > System > About

### Look for:

→ System type

* If it says "64-bit operating system, x64-based processor" 

 ### You're on AMD64

* If it says "64-bit operating system, ARM-based processor"
  
 ### You're on ARM64

### For Linux Os in the Cloud or WSL on Windows, follow this instructions:

* Linux: Use your distro's package manager

* sudo apt install docker.io
* sudo systemctl start docker
* sudo systemctl enable docker

* Make sure to verify your installation:

→ docker --version
→ docker run hello-world

## 4. Docker Basics

* Command	Description

* docker ps	List running containers
* docker images	List local images
* docker pull	Download image from Docker Hub
* docker run	Start container
* docker stop	Stop container

* docker run -d -p 80:80 nginx

* This runs NGINX in the background and maps port 80.

## 5. Dockerfile: Build Your Own Image

* Create a file called Dockerfile:

### Dockerfile, some examples:

**A simple Dockerfile for Node.js**
```
FROM node:alpine

# Copy the files from the host file system to the image file system
COPY . /app

# Set the working directory in the image
WORKDIR /app

# Run a command to start the application
CMD node app.js 
```
**A Complex Dockerfile for Multi-stage **

```
# Stage 1: Build dependencies
FROM node:18-alpine AS builder

WORKDIR /app

# Install dependencies
COPY package*.json ./
RUN npm install --production=false

# Copy source code
COPY . .

# Optional: Run tests or build steps here
# RUN npm test
# RUN npm run build

# Stage 2: Production image
FROM node:18-alpine

# Set environment variables
ENV NODE_ENV=production
ENV PORT=3000

WORKDIR /app

# Create a non-root user
RUN addgroup -S appgroup && adduser -S appuser -G appgroup

# Copy only necessary files from builder
COPY --from=builder /app /app

# Install only production dependencies
RUN npm ci --only=production

# Set permissions
RUN chown -R appuser:appgroup /app

USER appuser

# Expose port
EXPOSE 3000

# Healthcheck
HEALTHCHECK --interval=30s --timeout=10s --start-period=10s --retries=3 \
  CMD node -e "require('net').connect(process.env.PORT, '127.0.0.1').on('error', () => process.exit(1))"

# Start the application
CMD ["node", "app.js"]

```
**A Python Dockerfile**

```
FROM python:3.10
COPY . /app
WORKDIR /app
RUN pip install -r requirements.txt
CMD ["python", "app.py"]

* Then build and run:

* docker build -t my-python-app .
* docker run -p 5000:5000 my-python-app

## 6. Docker Compose (COMPLETELY Optional, but you should knbow it exist)

* Use Compose for multi-container apps:

version: '3'
services:
  web:
    build: .
    ports:
      - "5000:5000"
  db:
    image: postgres
    
* Run it:

* docker-compose up
```

## What to keep in mind:

* Learn about volumes (persistent data) (You will learn in in our Kubernetes program)

* Explore networks (inter-container communication) (You will learn in in our Kubernetes program)

* Push your images to Docker Hub

## Virtual Machines(VM) VS Containers

| Feature                | Virtual Machines (VMs)                                   | Containers                                         |
|------------------------|---------------------------------------------------------|----------------------------------------------------|
| **Definition**         | Emulates a full computer system with its own OS, running on a hypervisor above the host hardware | Runs applications in isolated environments that share the host OS kernel |
| **OS Usage**           | Each VM has its own complete guest operating system      | Containers share the host OS kernel, no separate OS |
| **Resource Usage**     | Consumes more memory and storage                        | Lightweight, uses less memory and disk space        |
| **Startup Time**       | Slower to boot                                          | Starts up almost instantly                          |
| **Isolation**          | Strong isolation, better hardware emulation             | Process-level isolation, less overhead              |
| **Use Case**           | Good for running different operating systems on one host | Best for microservices, rapid deployment, scaling   |

---
- **VMs:** Each application runs inside its own OS, managed by a hypervisor.
- **Containers:** Applications share the host OS kernel, making them more efficient and faster to start.

### Diagram: Virtual Machines vs Containers

![image](https://github.com/user-attachments/assets/29c0e35a-83c3-4686-acd8-2edabe182046)


### What is a Kernel?

***Definition:*** 

- The kernel is the central component of an operating system, responsible for handling interactions between software and hardware.

***Description:***

- It manages essential functions such as memory allocation
-  Process scheduling
-  File system operations, and
-  Device input/output. 

### Critical notice

### Applications access hardware resources through the kernel rather than directly for several important reasons:

- **Safety and Security:** The kernel prevents applications from interfering with each other or the system, protecting data and hardware from unauthorized access or damage.
  
- **Resource Management:** The kernel manages and allocates hardware resources (CPU, memory, devices) efficiently among all running applications.

- **Abstraction:** The kernel provides a consistent interface for applications, so they don’t need to handle hardware differences or complexities.
  
- **Stability:** By controlling hardware access, the kernel helps prevent crashes and system instability caused by faulty or malicious applications.

## The kernel acts as a protective and organizing layer between applications and hardware. Take a look at the diagram below

![image](https://github.com/user-attachments/assets/54142db8-4d77-4139-920e-1180cb16b792)

## A comprehensive table comparing Docker and Kubernetes, including their definitions, advantages, disadvantages, how they work together, and the problems they solve:

| Aspect                | Docker                                                                                      | Kubernetes                                                                                   |
|-----------------------|--------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------|
| **Definition**        | Platform for building, packaging, and running applications in containers                   | Container orchestration system for automating deployment, scaling, and management of containers|
| **Primary Role**      | Containerization: creates and runs containers                                              | Orchestration: manages containers across multiple hosts (cluster)                             |
| **Scope**             | Single host                                                                                | Multi-host (cluster)                                                                          |
| **Advantages**        | - Portability<br>- Consistency<br>- Isolation<br>- Lightweight<br>- Fast startup           | - Automated scaling<br>- Self-healing<br>- Load balancing<br>- Rolling updates/rollbacks<br>- Multi-host management |
| **Disadvantages**     | - Limited to single host<br>- Manual scaling<br>- No built-in orchestration                | - Steep learning curve<br>- More complex setup<br>- Higher resource and operational overhead  |
| **Problem Solved**    | Simplifies packaging, shipping, and running applications in a consistent environment       | Automates deployment, scaling, and management of containers for high availability and resilience|
| **How They Work Together** | Docker builds and runs containers; Kubernetes deploys, scales, and manages those containers across clusters | Docker containers are orchestrated by Kubernetes for production-grade deployments             |
| **Typical Workflow**  | 1. Write code<br>2. Create Dockerfile<br>3. Build image<br>4. Run container<br>5. Test/debug<br>6. Tag/push image<br>7. Deploy | 1. Build Docker image<br>2. Push to registry<br>3. Define Kubernetes manifests<br>4. Deploy to cluster<br>5. Kubernetes manages scaling, updates, and health checks |
| **Use Case**          | Packaging and running single or a few containers on a developer’s machine or server        | Managing, scaling, and operating many containers in production environments                   |

---

## How They Work Together:

- ### Docker
  
 **It is used to package and run your application in containers.**

- ### Kubernetes

  **It is used to deploy, scale, and manage those containers across a cluster of machines.**

---

**Summary Table**

| Feature            | Docker                                   | Kubernetes                                 |
|--------------------|------------------------------------------|--------------------------------------------|
| Purpose            | Containerization platform                | Container orchestration platform           |
| Scope              | Single host                              | Multi-host (cluster)                       |
| Scaling            | Manual                                   | Automated                                  |
| Self-Healing       | No                                       | Yes                                        |
| Load Balancing     | No (basic only)                          | Yes                                        |
| Complexity         | Simple                                   | Complex                                    |
| Use Case           | Packaging and running containers         | Managing, scaling, and deploying containers|

---


## Detailed side-by-side comparison of **Docker Swarm** and **Kubernetes**:

| Aspect                | Docker Swarm                                                                 | Kubernetes                                                                 |
|-----------------------|------------------------------------------------------------------------------|----------------------------------------------------------------------------|
| **Definition**        | Native Docker orchestration tool for clustering and managing containers      | Advanced open-source orchestration platform for managing container clusters |
| **Setup Complexity**  | Simple and quick to set up                                                   | More complex, requires more configuration                                  |
| **Scalability**       | Good for small to medium clusters                                            | Designed for large-scale, production-grade clusters                        |
| **Load Balancing**    | Built-in, basic load balancing                                               | Advanced load balancing and service discovery                              |
| **Self-Healing**      | Limited (restarts failed containers)                                         | Robust (auto-replaces, restarts, and reschedules containers)               |
| **Rolling Updates**   | Supported, but less advanced                                                 | Advanced rolling updates and rollbacks                                     |
| **Networking**        | Simple overlay networking                                                    | Advanced networking options (e.g., network policies, ingress controllers)  |
| **Storage**           | Basic volume support                                                         | Advanced persistent storage management                                     |
| **Monitoring & Logging** | Basic, requires third-party tools                                         | Extensive ecosystem and integrations for monitoring/logging                |
| **Community & Ecosystem** | Smaller, maintained by Docker                                            | Large, active community and ecosystem                                      |
| **Use Case**          | Simpler orchestration needs, quick deployments, smaller teams                | Enterprise-level, complex, large-scale deployments                         |
| **Learning Curve**    | Gentle                                                                       | Steep                                                                      |

---

***How They Work Together with Docker:***

- Both use Docker containers as the basic unit of deployment.
- **Docker Swarm** is tightly integrated with Docker CLI and is easier for those already using Docker.
- **Kubernetes** can run Docker containers (and others), but requires more setup and offers more features for complex environments.

---

**Summary:**  

## Docker Swarm

**It is best for simple, quick, and smaller-scale container orchestration.**
 
## Kubernetes

**It is suited for complex, large-scale, and production-grade orchestration with advanced features.**



## Each step in the Docker development workflow:

1. ***Developers write code:***
   
   **Developers develop the application using the preferred programming language and tools.**

2. ***Create a Dockerfile:***

   Developers write a `Dockerfile` that describes how to package the application, including dependencies, build steps, and how to run it.

3. ***Build the Image:***  

   Using the command `docker build`, you create a Docker image based on your Dockerfile. This image contains everything needed to run your app.

4. ***Run the Container:***  

   You start a container from your image with `docker run`. This launches your app in an isolated environment.

5. ***Test and Debug:***  

   You test your application inside the container. If you find issues, you update your code or Dockerfile, then rebuild and rerun the container.

6. ***Tag and Push Image:***  

   Once your app works as expected, you tag the image and push it to a container registry (like Docker Hub) so others can access it.

7. ***Deploy:***  

   In production or staging, you pull the image from the registry and run it as a container, ensuring consistency across environments.

***This workflow helps develop, test, and deploy applications efficiently and reliably using Docker.***

## Workflow Commands

**Please, make sure before doing anything, your Docker engine is running, you are signed into your Docker desktop, and it's the same account for both environments.**

## The complete Docker development workflow with all key commands:

1. **Log in to Docker Hub (or your registry):**
   ```
   docker login
   ```

2. **Build your Docker image:**
   ```
   docker build -t username/your-image-name:tag .
   ```

3. **Run a container from your image:**
   ```
   docker run -p 8080:80 username/your-image-name:tag
   ```

4. **List running containers:(Optional)**
   ```
   docker ps
   ```

5. **Stop a running container:**
   ```
   docker stop <container_id>
   ```

6. **Tag your image (if needed):**
   ```
   docker tag your-image-name username/your-image-name:tag
   ```

7. **Push your image to Docker Hub:**
   ```
   docker push username/your-image-name:tag
   ```

8. **Pull your image from Docker Hub (on another machine or server):**
   ```
   docker pull username/your-image-name:tag
   ```

Replace `username`, `your-image-name`, `tag`, and `<container_id>` with your actual Docker Hub username, image name, tag, and container ID.

## Here are the Docker commands to delete both containers and images:

## Best option to delete both the container and the image at the same time, pass the -f flag:

```
docker rmi -f <image-name-or-id>

```

***That will forcibly remove the image and any stopped containers using it, but first, make sure to stop the running container***

### Delete a Container

**Delete a specific container:**

***docker rm <container_name_or_id>***

```
docker rm bmt-test-container
```

**First, stop it if it's running:**

***docker stop <container_name_or_id>***

**Example**

```
docker rm bmt-test-container
```
**Delete all stopped containers:**
```
docker container prune
```
## Delete a Docker Image

**Delete a specific image:**

***docker rmi <image_name_or_id>***

***Example:***
```
docker rmi nginx
```
## Delete all unused images:
```
docker image prune
```
## You may run into some errors:

**Example**

BMT@Eta-Ray ~/bmt-docker/Docker/projects/app.js-docker-project (master)$ docker rmi e41698f75c81
Error response from daemon: conflict: unable to delete e41698f75c81 (must be forced) - image is being used by stopped container 67af49674e25

***The error you're seeing:***

**Error response from daemon: conflict: unable to delete e41698f75c81 (must be forced) - image is being used by stopped container 67af49674e25**
**means Docker won’t delete the image because a container (even if stopped) is still using it.**

## Solution to remove the container: Remove the stopped container first

```
docker rm 67af49674e25
```

***Then try deleting the image again:***

```
docker rmi e41698f75c81
```

## Alternative: Force delete the image

**If you want to force-delete the image and remove all associated containers automatically (⚠️ destructive):**

```
docker rmi -f e41698f75c81
```

## Notice: Use this only if you're sure you don’t need the container. For practice purposes, it doesn't matter.

## Importance:

### How do I know which particular container an image is using?

 ***To find which containers are using a specific Docker image, you can use this command, and please DO NOT TRY TO MEMORISE THESE COMMANDS.***

 **IF YOU TRY THIS COMMAND AND GET NO RESULTS, TRY THE ALTERNATIVE APPROACH BELOW**

```
docker ps -a --filter ancestor=<image-id-or-name>
```

```
docker ps -a --filter ancestor=0b40d2c78646
```

***This shows all containers (running or stopped) that were created from that image.***

## Alternatively, use this broader approach:

**List all containers with their image:**

```
docker ps -a --format "table {{.ID}}\t{{.Image}}\t{{.Status}}"
```

***Then, visually match the IMAGE column with your image ID or name.***

## You can also see how much space Docker is currently using in your system by running:

```
docker system df
```

***This command shows disk usage by images, containers, and volumes.***


## To clean up unused data, you can run:

```
docker system prune
```

*(This will remove unused images, containers, and networks. Use with caution.)*



## To reclaim this space, run:

```
docker system prune -a --volumes
```

*(This will remove all unused images, containers, and volumes. Use with caution!)*

## Follow this link to start practicing how to build an image using Docker:

```
https://github.com/rayeeta/Docker
```

***Steps***

- **Click on projects**
  
- **Navigate to app.js-docker-project.**  In this directory are the project files Dockerfile and app.js

### Please, make sure to mkdir <give-it-your-own-name>, cd to this new directory, and clone this repository inside the new directory, cd to the project directory 'app.js-docker-project.' You can start building from here





   #####################################################################################################
   ###                             You must be feeling like a champ already, right?                                                   
                                                                                                                                              
   ##                              If you found this repo useful, give it a STAR 🌠                                                  
   #####################################################################################################




