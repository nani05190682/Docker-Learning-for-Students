# Docker

Docker is an open-source platform that automates the deployment, scaling, and management of applications. It uses containerization technology to bundle an application and its dependencies into a single object, called a Docker container. These containers can then be distributed and run on any system that supports Docker, regardless of the underlying host operating system.

Here's an analogy: if a traditional virtual machine is like a house, which has its own infrastructure, a Docker container is like an apartment in an apartment building, which shares some common infrastructure.

This leads us to some of the key benefits and use cases of Docker:

1. **Simplified Configuration:** Docker lets you set up your applications, dependencies, and environment variables in a script. This is like a blueprint for your application, which can be run anywhere Docker is installed, reducing the "it works on my machine" problem.

2. **Code Pipeline Management:** Docker can simplify continuous integration, testing, and deployment workflows. For instance, your build server can create a Docker image (the blueprint for a container), and the same image can be used across every step of the release process.

3. **Isolation:** Docker containers run in isolation from each other, which improves security and reduces conflicts between different applications or different instances of the same application.

4. **Rapid Deployment:** Containers are very lightweight and start quickly. This makes Docker great for deploying apps and for scaling in response to load, especially in cloud environments.

5. **Version Control and Component Reuse:** Docker includes versioning capabilities. You can track versions of a Docker image, roll back to previous versions, and reuse components between containers.

6. **Debugging Capabilities:** You can use Docker to create an image of a problem for troubleshooting. Once the problem is fixed, you can update the Docker image without affecting the rest of your application.

7. **Multi-Platform Deployment:** Docker can run on many different systems, from your laptop to the cloud, and can be used for tasks ranging from setting up developers' environments to hosting your applications in production.

## What happens in the background when we run a container?

When you run a Docker container, several things happen behind the scenes:

1. **Image Retrieval:** Docker checks if the image is available locally. If not, it's downloaded from the specified Docker registry (like Docker Hub).

2. **Container Creation:** Docker creates a new container based on the image, adding a read-write layer on top of the read-only image.

3. **Filesystem Setup:** Docker sets up a union filesystem, allowing multiple filesystems to be mounted at once. Write operations are recorded at the top layer (copy-on-write system).

4. **Networking:** Docker connects the container to a network, assigning an IP address and setting up port forwarding.

5. **Process Management:** Docker starts the specified process from the Dockerfile, and the container's life is tied to this process.

6. **Logging:** Docker captures output from the container's process (stdout and stderr) and can forward it for viewing.

7. **Resource Management:** Docker enforces specified resource constraints such as CPU and memory limits.

8. **Inter-Container Communication:** Docker sets up inter-process communication if needed.

The result is a running container that you can interact with like a virtual machine, but much lighter-weight.

## What is the difference between a container and VM?

**Virtual Machines (VMs):**
- Abstraction of an entire computer with its own operating system (Guest OS).
- Runs on physical hardware through a hypervisor (Host OS).
- Each VM includes a full copy of an operating system, application, binaries, and libraries.
- Less efficient in terms of system resource usage.
- Slower to boot.
- Provides hardware-level isolation.

**Containers:**
- Packages an application along with its runtime, libraries, and system tools in a single unit (container image).
- Runs on the host system's OS kernel directly, without needing its own OS.
- Shares the host system's OS kernel, binaries, and libraries.
- More lightweight, starts quickly.
- More efficient in terms of system resources.
- Provides isolation through Linux namespaces, cgroups, and OS-level features.

In essence, VMs virtualize the hardware to run isolated operating systems, while containers virtualize the operating system to run isolated applications.

## First Container

Creating an NGINX container with Docker is straightforward and can be accomplished with a few commands.

1. **Pull the NGINX Image from Docker Hub:**
   ```bash
   docker pull nginx
   ```

2. **Run the NGINX Container:**
   ```bash
   docker run --name my-nginx -p 8080:80 -d nginx
   ```
   - `--name my-nginx`: assigns a name to the container.
   - `-p 8080:80`: maps container port 80 to host port 8080.
   - `-d`: runs the container in detached mode.

Now, access the NGINX server at [http://localhost:8080](http://localhost:8080) in your browser.

## Verify the running Containers

To view the list of running Docker containers:
```bash
docker ps
```
To see all containers (including stopped ones):
```bash
docker ps -a
```
To display only the numeric IDs of containers:
```bash
docker ps -q
```
Combine options as needed, e.g., to display IDs of all containers:
```bash
docker ps -aq
```

## Monitoring Docker Container

Docker provides commands for monitoring container status and resource usage:

- **Docker stats command:**
  ```bash
  docker stats
  ```
  - For a specific container:
  ```bash
  docker stats <container_id>
  ```

- **Docker logs command:**
  ```bash
  docker logs <container_id>
  ```

- **Docker inspect command:**
  ```bash
  docker inspect <container_id>
  ```

- **Docker events command:**
  ```bash
  docker events
  ```

- **External Monitoring Tools:** Use tools like Prometheus, Grafana, Datadog, New Relic, Sysdig, etc., for more advanced monitoring.

## Getting inside the container

To get inside a running Docker container:
```bash
docker exec -it <container_id> /bin/bash
```
- `-it`: Interactive mode with a pseudo-TTY.
- `/bin/bash`: Launches a Bash shell inside the container.

Exit the container with `exit` or `Ctrl+D`.

## Docker Prune

The `docker prune` command cleans up unused Docker objects (containers, images, networks, volumes).

Examples:

- Remove stopped containers, dangling images, and unused networks:
  ```bash
  docker system prune
  ```

- Remove all unused containers:
  ```bash
  docker container prune
  ```

- Remove all unused images:
  ```bash
  docker image prune
  ```

- Remove all unused networks:
  ```bash
  docker network prune
  ```

- Remove all unused volumes:
  ```bash
  docker volume prune
  ```

## Lab Exercise

### Exercise 1

1. Create and stop three Docker containers from the busybox image.
   ```bash
   docker run --name test-container-1 busybox
   docker run --name test-container-2 busybox
   docker run --name test-container-3 busybox
   ```



2. Check the list of all containers (including stopped ones).
   ```bash
   docker ps -a
   ```

3. Use the Docker prune command to remove all stopped containers.
   ```bash
   docker container prune
   ```

4. Verify that the stopped containers have been removed.
   ```bash
   docker ps -a
   ```

### Exercise 2

1. List all Docker containers, images, volumes, and networks.
   ```bash
   docker ps -a
   docker images
   docker volume ls
   docker network ls
   ```

2. Run the Docker system prune command to remove unused resources.
   ```bash
   docker system prune
   ```

3. Verify that the unused resources have been removed.
   ```bash
   # Repeat the commands from step 1
   ```

## Conclusion

In this guide, you learned about Docker, the background processes when running a container, the difference between containers and VMs, creating and monitoring containers, accessing container internals, using Docker prune, and practical lab exercises. Docker's networking model was also briefly introduced.
