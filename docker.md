Docker

Docker is an open-source platform that automates the deployment, scaling, and management of applications. It uses containerization technology to bundle an application and its dependencies into a single object, called a Docker container. These containers can then be distributed and run on any system that supports Docker, regardless of the underlying host operating system.

Here's an analogy: if a traditional virtual machine is like a house, which has its own infrastructure, a Docker container is like an apartment in an apartment building, which shares some common infrastructure.

This leads us to some of the key benefits and use cases of Docker:

1. Simplified Configuration: Docker lets you set up your applications, dependencies, and environment variables in a script. This is like a blueprint for your application which can be run anywhere Docker is installed, reducing the "it works on my machine" problem.

2. Code Pipeline Management: Docker can simplify continuous integration, testing, and deployment workflows. For instance, your build server can create a Docker image (the blueprint for a container) and the same image can be used across every step of the release process.

3. Isolation: Docker containers run in isolation from each other, which improves security and reduces conflicts between different applications or different instances of the same application.

4. Rapid Deployment: Containers are very lightweight and start quickly. This makes Docker great for deploying apps and for scaling in response to load, especially in cloud environments.

5. Version Control and Component Reuse: Docker includes versioning capabilities. You can track versions of a Docker image, roll back to previous versions, and reuse components between containers.

6. Debugging Capabilities: You can use Docker to create an image of a problem for troubleshooting. Once the problem is fixed, you can update the Docker image without affecting the rest of your application.

7. Multi-Platform Deployment: Docker can run on many different systems, from your laptop to the cloud, and can be used for tasks ranging from setting up developers' environments to hosting your applications in production.

What happens in the background when we run a container ?

When you run a Docker container, several things happen behind the scenes:

Image Retrieval: Docker first checks if the image you want to run is available locally. If it is not, Docker will download it from the specified Docker registry (like Docker Hub). A Docker image is like a blueprint for your Docker container. It contains everything that will be needed to run your container - the code, runtime, libraries, environment variables, and config files.

Container Creation: Docker creates a new container based on the image. It does this by creating a read-write layer on top of the read-only image, and then initializing various settings - network ports, storage options, container name, ID, and more.

Filesystem Setup: Docker sets up a union filesystem. This type of filesystem allows several filesystems to be mounted at once, but appear to be one filesystem. The lower layers are read-only, and the top layer is read-write. When a write is performed, it is only recorded at the top layer (this is called a "copy-on-write" system).

Networking: Docker connects the container to a network. Unless told otherwise, it's the default bridge network. It assigns an IP address to the container, and sets up rules for port forwarding, so that the container can communicate with other containers and with the outside world.

Process Management: Docker then starts the process you specified in the Dockerfile (the file that describes what goes in the image). The container's life is tied to the life of this process - if the process ends, the container ends.

Logging: Docker starts capturing output from the container's process, usually both standard output (stdout) and standard error (stderr), and it can forward these somewhere for you to view.

Resource Management: Docker also enforces any resource constraints that you've specified, such as CPU and memory limits.

Inter-Container Communication: Docker sets up IPC (inter-process communication) if the container is supposed to communicate with other containers.

The result is a running container that you can interact with just like a virtual machine, except it's much lighter-weight than a VM. This entire process happens very quickly, which is part of what makes Docker so useful for development, testing, and deployment scenarios.

What is the difference between a container and VM ?

Containers and Virtual Machines (VMs) both serve to provide an isolated environment for running applications, but they do so in very different ways.

Virtual Machines (VMs):

A VM is an abstraction of an entire computer - it has its own operating system (known as the Guest OS), and it runs on the physical hardware through a layer called the hypervisor (which can be considered as a "Host" OS).
VMs are isolated at the hardware level by the hypervisor.
Each VM includes a full copy of an operating system, the application, necessary binaries, and libraries - taking up tens of GBs. VMs can also be slow to boot.
VMs are generally less efficient than containers in terms of system resource usage because they run a full OS stack in addition to the application.
Containers:

A container packages an application along with its runtime, libraries, system tools, and code in a single unit (a container image) and runs on the operating system's kernel directly. It doesn't need its own operating system, unlike a VM.
Containers share the host system's OS kernel and, usually, the binaries and libraries, too. They run as isolated processes in userspace on the host operating system.
Containers are isolated with each other and with the host system by way of Linux namespaces, cgroups, and other OS-level features.
They are more lightweight and start much faster than VMs, as they don't have the overhead of a full OS. A container might be only tens of MBs in size, and start in seconds.
Containers are generally more efficient than VMs in terms of system resources. Several more containers can be run on a host machine compared to VMs.
In essence, the main difference is that VMs virtualize the hardware, allowing you to run multiple full-stack, isolated operating systems on a single machine, while containers virtualize the operating system, allowing you to run multiple isolated applications on a single OS. Each approach has its use cases. For example, if you want full isolation with dedicated resources, you might choose a VM. If you want to maximize the number of applications running on a minimal number of servers, you might choose containers.

First Container

Creating an NGINX container with Docker is straightforward and can be accomplished with a few commands.

Pull the NGINX Image from Docker Hub: Docker Hub is a cloud-based registry service which allows you to link to code repositories, build your images, store manually pushed images, and test your images. NGINX official Docker images are hosted there. To pull the NGINX image, use the following command:

docker pull nginx
This command will download the latest version of the NGINX image. If you want a specific version, you can specify it, like nginx:1.17.

Run the NGINX Container: After the image has been downloaded, you can create a new container and run it:

docker run --name my-nginx -p 8080:80 -d nginx
Here's what this command does:

run tells Docker to start a new container
--name my-nginx assigns the name my-nginx to your container. You can choose any name you want.
-p 8080:80 maps port 80 in the container (the default HTTP port for NGINX) to port 8080 on your Docker host. You can choose a different port on the host if 8080 is already in use.
-d tells Docker to run the container in the background (detached mode)
nginx is the name of the image to base the container on
Now, you can access your NGINX server by navigating to http://localhost:8080 in your browser (or http://your-docker-host-ip:8080 if you're running Docker on a different machine).

Remember, this is a very basic setup and doesn't involve any customization of the NGINX configuration or serving any specific files. For a more complex setup, you would likely want to build your own Docker image, based on the NGINX image but adding your own configuration files or website data. This typically involves creating a Dockerfile and using the docker build command.

Verify the running Containers:

To view the list of running Docker containers, you can use the following command:

docker ps
This command displays information about all currently running Docker containers including their IDs, image names, created time, status, ports, and names.

If you want to see all containers (not just the running ones), including those that have been stopped, you can use the -a or --all option like so:

docker ps -a
Another useful option is -q or --quiet which only displays the numeric IDs of the containers:

docker ps -q
You can combine these options as needed, for example:

docker ps -aq
This command would display the IDs of all containers (both running and stopped). These IDs can be used in other Docker commands to manage the containers (e.g., to stop a running container, remove a container, etc.).

Monitoring Docker Container

Monitoring Docker containers is essential for troubleshooting and maintaining healthy applications. Docker provides built-in commands to monitor the status and resource usage of your containers. Beyond this, more advanced monitoring can be performed using external tools. Here are some options:

Docker stats command: This command displays a live stream of CPU usage, memory usage, network IO, block IO, PIDs, etc., for all running containers or for a specific container if specified.

docker stats
If you want to see stats for a specific container, you can do so by specifying the container:

docker stats <container_id>
Docker logs command: This command allows you to view the logs for a container. It can be useful for troubleshooting problems and verifying application behavior.

docker logs <container_id>
Docker inspect command: This command provides more detailed low-level information about a specific container or image.

docker inspect <container_id>
This command returns a JSON object that includes configuration, state, network information, and more about the container.

Docker events command: This command shows real-time events such as the start, stop, kill, etc., of containers.

docker events
External Monitoring Tools: For more advanced or detailed monitoring, or if you're running a large number of containers, you may need to use a third-party Docker monitoring tool. These tools can provide more in-depth insight and visualizations, alerting, long-term data retention, and more.

Some popular tools for Docker monitoring include Prometheus (often used in combination with Grafana for visualization), Datadog, New Relic, Sysdig, etc. These tools can monitor metrics such as CPU and memory usage, network usage, disk I/O, etc., and also allow you to set up alerts based on these metrics.

Please remember, it's crucial to assess your requirements and select a monitoring strategy or tool that aligns with your use case. If you are using orchestration tools like Kubernetes or Docker Swarm, you will need to consider monitoring at the cluster level as well.

Getting inside the container:

To get inside a running Docker container, you can use the docker exec command along with an interactive shell like bash. Here's an example:

docker exec -it <container_id> /bin/bash
Here's what this command does:

exec is a Docker command that allows you to run specific commands in a running container.
-it option makes the command interactive (i.e., it lets you type or input commands directly), and allocates a pseudo-TTY for the command. -i stands for --interactive, and -t stands for --tty.
<container_id> is the ID of the container you want to enter. You can get the container ID by running docker ps.
/bin/bash launches a Bash shell inside the container.
After running this command, you'll be inside the container and able to execute commands as if you were logged into a system with that container's file system.

Please note, some Docker images may not have a Bash shell installed. If Bash is not available, you can use sh instead:

docker exec -it <container_id> /bin/sh
You can exit the container by typing exit or pressing Ctrl+D.

Docker Prune

The docker prune command is used to clean up unused Docker objects such as containers, images, networks, and volumes. It can help free up storage space by removing objects that are no longer needed.

Here are some examples of the docker prune command:

Remove all stopped containers, all dangling images, and all unused networks:

docker system prune
If you want to also remove any unused images (not just dangling ones), you can use the -a (or --all) flag:

docker system prune -a
Remove all unused containers:

docker container prune
Remove all unused images:

docker image prune
And to remove all unused images, not just the dangling ones, you can use the -a (or --all) flag:

docker image prune -a
Remove all unused networks:

docker network prune
Remove all unused volumes:

docker volume prune
Lab Exercise

lab exercise on the usage of docker prune commands along with their solutions:

Exercise:

Start by creating a new Docker container from the busybox image and stop it immediately. Repeat this process three times.
Check the list of all containers, including the stopped ones.
Use a Docker prune command to remove all stopped containers.
Check the list of all containers again to verify that the stopped containers have been removed.
Solution:

Creating and stopping the containers:
docker run --name test-container-1 busybox
docker run --name test-container-2 busybox
docker run --name test-container-3 busybox
Each of these commands will pull the busybox image (if it isn't already available), create a new Docker container with a specified name, and then stop it.

Check the list of all containers:
docker ps -a
This command should display all containers, including the three that you just created and stopped.

Remove all stopped containers:
docker container prune
You'll need to confirm the action by entering y or yes. This command removes all stopped containers.

Verify that the stopped containers have been removed:
docker ps -a
This command should now show no containers (or at least, not the ones you just created), confirming that the docker container prune command successfully removed all stopped containers.

Please note that docker prune commands should be used carefully, especially in production environments, as they can permanently delete Docker objects.

Exercise2 Lab exercise for the docker prune command:

Lab Exercise: Using the docker prune Command
Objective
In this exercise, you will learn how to use the docker prune command to remove unused Docker resources.

Prerequisites
Docker installed on your machine
Basic understanding of Docker commands
Steps
Open a terminal window and run the following command to list all of the Docker containers on your machine:

docker ps -a
This will list all of the containers, including those that are stopped or exited.

Run the following command to list all of the Docker images on your machine:

docker images
This will list all of the images that are currently downloaded on your machine.

Run the following command to list all of the Docker volumes on your machine:

docker volume ls
This will list all of the volumes that are currently created on your machine.

Run the following command to list all of the Docker networks on your machine:

docker network ls
This will list all of the networks that are currently created on your machine.

Now, run the following command to remove all stopped containers, dangling images, unused networks, and unused volumes:

docker system prune
This will remove all of the unused resources, freeing up disk space on your machine.

Run the commands from steps 1-4 again to verify that the unused resources have been removed.

Conclusion
In this exercise, you learned how to use the docker prune command to remove unused Docker resources. This can help you free up disk space on your machine and ensure that you're only keeping the resources that you actually need.

Please be careful when using the prune commands, especially with the -a (or --all) flags, as it can lead to data loss if you have volumes or images that you intended to use later. Always make sure to properly tag and use Docker images, containers, and volumes to prevent unnecessary cleanups. Networking in Docker:

Docker has a robust networking model which supports various ways to manage network communications in containers. Here's a high-level overview:
