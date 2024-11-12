**Lab 1: Building a Basic Docker Image**

**Objective:** Create a simple Docker image that runs a shell command.

**Solution:**

1. Create a file named `Dockerfile` with the following content:

   ```dockerfile
   FROM ubuntu:latest
   RUN echo "Hello, Docker!"
   CMD ["/bin/bash"]
   ```

2. Build the image:

   ```bash
   docker build -t hello-world .
   ```

3. Run the image:

   ```bash
   docker run hello-world
   ```

   This will print "Hello, Docker!" and open a bash shell.

**Improvement:**

- Consider using a multi-stage build for smaller final images:

   ```dockerfile
   FROM ubuntu:latest AS builder
   RUN echo "Hello, Docker!"

   FROM alpine:latest
   CMD ["/bin/sh", "-c", "echo Hello, Docker!"]
   ```

**Lab 2: Installing and Running an Application**

**Objective:** Create a Docker image that installs and runs a web server (e.g., Apache).

**Solution:**

1. Create a `Dockerfile`:

   ```dockerfile
   FROM ubuntu:latest
   RUN apt-get update && apt-get install -y apache2
   EXPOSE 80
   CMD ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]
   ```

2. Build and run the image:

   ```bash
   docker build -t my-apache .
   docker run -d -p 80:80 my-apache
   ```

**Improvement:**

- Copy your web application code or configuration files into the image for customization.

**Lab 3: Using Environment Variables**

**Objective:** Create an image that sets environment variables and uses them within an application.

**Solution:**

1. Create a `Dockerfile`:

   ```dockerfile
   FROM python:latest
   WORKDIR /app
   ENV MY_VAR="Hello"
   COPY requirements.txt .
   RUN pip install -r requirements.txt
   COPY . .
   CMD ["python", "my_script.py"]
   ```

   (Replace `my_script.py` with your actual script that uses `MY_VAR`.)

2. Build and run:

   ```bash
   docker build -t my-app .
   docker run my-app
   ```

**Improvement:**

- Use `.env` files for managing sensitive environment variables outside the Dockerfile.

**Lab 4: Multi-Stage Builds for Smaller Images**

**Objective:** Create a multi-stage build to optimize image size.

**Solution:**

1. Create a `Dockerfile`:

   ```dockerfile
   FROM node:18 AS builder
   WORKDIR /app
   COPY package.json .
   RUN npm install

   FROM node:alpine
   WORKDIR /app
   COPY --from=builder /app/node_modules .
   COPY . .
   CMD ["npm", "start"]
   ```

2. Build and run:

   ```bash
   docker build -t my-node-app .
   docker run -d -p 3000:3000 my-node-app
   ```

**Improvement:**

- Explore tools like `buildkit` for even more optimized builds.

**Lab 5: Copying Application Code**

**Objective:** Create an image that copies your application code into the image.

**Solution:**

1. Create a directory structure for your application (e.g., `my-app`).

2. Create a `Dockerfile`:

   ```dockerfile
   FROM python:3.9
   WORKDIR /app
   COPY requirements.txt .
   RUN pip install -r requirements.txt
   COPY . .
   CMD ["python", "main.py"]
   ```

3. Build from the directory containing the `Dockerfile` and your application code:

   ```bash
   docker build -t my-app .
   ```

**Improvement:**

- Use Git for version control and build the image from your Git repository.

**Lab 6: Running a Background Process**

**Objective:** Create an image that runs a program in the background.

**Solution:**

1. Create a `Dockerfile`:

   ```dockerfile
   FROM python:latest
   WORKDIR /app
   COPY requirements.txt .
   RUN pip install -r requirements.txt
   COPY worker.py .
   CMD ["python", "worker.py"]
   ```

