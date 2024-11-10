Creating a custom Docker image for nginx involves writing a Dockerfile. Here's an example of how you might create a simple Dockerfile that creates a custom nginx image with a custom HTML file.

**Step 1:** Create a new directory, navigate into it and create a new Dockerfile:

```bash
mkdir nginx-custom
cd nginx-custom
touch Dockerfile
```

**Step 2:** Open the Dockerfile in a text editor and add the following content:

```Dockerfile
# Start from the nginx:latest image
FROM nginx:latest

# Remove the default nginx index page
RUN rm /usr/share/nginx/html/index.html

# Add a new custom index page
COPY custom-index.html /usr/share/nginx/html/index.html
```

In the same directory, create a new file called `custom-index.html` with the following content:

```html
<!DOCTYPE html>
<html>
<body>

<h1>Welcome to my Custom Nginx Server!</h1>

</body>
</html>
```

**Step 3:** Build your Docker image:

```bash
docker build -t nginx-custom .
```

This command tells Docker to build an image using the Dockerfile in the current directory (`.`) and tag it as `nginx-custom`.

**Step 4:** Verify that your image has been built:

```bash
docker images
```

You should see `nginx-custom` in the list of images.

**Step 5:** Now, you can run a container using your custom image:

```bash
docker run -d -p 8080:80 nginx-custom
```

This command tells Docker to run a new container in the background (`-d`) using the `nginx-custom` image, mapping port 8080 of your host to port 80 in the container.

**Step 6:** Finally, open your browser and go to `http://localhost:8080`. You should see "Welcome to my Custom Nginx Server!".

That's it! You've created and run a custom Docker image for nginx.