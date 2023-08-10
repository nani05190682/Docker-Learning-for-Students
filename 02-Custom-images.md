Building a custom Docker image involves creating a Dockerfile, which is a text file that contains a list of commands that Docker can call to assemble an image. 

Here are the steps you can follow to build a custom Docker image:

**Step 1:** Create a Dockerfile in a new directory.

```bash
mkdir my_custom_image
cd my_custom_image
touch Dockerfile
```

**Step 2:** Open the Dockerfile in a text editor and define it.

Here is an example Dockerfile that will create a simple Apache server with a custom HTML file:

```Dockerfile
# Start with an existing image as a base
FROM httpd:2.4

# Copy a file from the local host to the filesystem of the image
COPY ./index.html /usr/local/apache2/htdocs/
```

In the same directory as the Dockerfile, you would create an `index.html` file with the content you want, for example:

```html
<!DOCTYPE html>
<html>
<body>

<h1>Hello World from my Custom Docker Image!</h1>

</body>
</html>
```

**Step 3:** Build the Docker image using the `docker build` command. The `-t` flag lets you tag your image so it's easier to find later:

```bash
docker build -t my_custom_image .
```

The `.` at the end of the command tells Docker to use the current directory as the context.

**Step 4:** After building, you can check that the image is available with the following command:

```bash
docker images
```

You should see your image listed.

**Step 5:** To use the image, you can run a new container with the `docker run` command:

```bash
docker run -d -p 8080:80 my_custom_image
```

In this command, `-d` runs the container in detached mode (in the background), `-p` maps the host's port 8080 to the container's port 80, and `my_custom_image` is the image to use to create the container.

Now, if you open a web browser and go to `http://localhost:8080`, you should see "Hello World from my Custom Docker Image!"

This is a basic example. Dockerfiles can get much more complex depending on your needs. You can install software, clone repositories, set environment variables, expose ports, etc. All of these are done using various Dockerfile instructions.


