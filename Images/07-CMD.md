### **3.9. CMD (Default Command)**

The **`CMD`** directive in a Dockerfile is used to specify the default command that should be executed when a Docker container starts. Unlike the `RUN` directive, which executes commands during the build process, `CMD` is executed when the container is launched.

---

#### **Purpose of `CMD`:**
- Defines the default command and arguments for the container.
- There can only be one `CMD` instruction in a Dockerfile. If you include multiple `CMD` instructions, only the last one will take effect.
- The command defined in `CMD` can be overridden by passing a different command when running the container (`docker run <image> <command>`).

---

#### **Usage:**
```dockerfile
CMD ["executable", "param1", "param2"]
```

- Uses the **exec form** (preferred) which is specified as a JSON array.
  - Example: `CMD ["nginx", "-g", "daemon off;"]`

Alternatively, you can use the **shell form**:
```dockerfile
CMD command param1 param2
```
- Example: `CMD nginx -g 'daemon off;'`
- Note: The shell form runs the command in a shell (`/bin/sh -c`), which can lead to unexpected behaviors. Therefore, the exec form is recommended for better performance and reliability.

---

### **Example: Setting a Default Command to Run an Nginx Web Server**

```dockerfile

FROM nginx:latest
COPY index.html /usr/share/nginx/html/index.html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

---

### **How `CMD` Differs from `ENTRYPOINT`**

- `CMD` is often used to provide **default arguments** to an `ENTRYPOINT` instruction.
- If both `CMD` and `ENTRYPOINT` are specified, `CMD` acts as the default parameters for the executable defined in `ENTRYPOINT`.
  
**Example**:
```dockerfile
# Using both ENTRYPOINT and CMD
FROM ubuntu:20.04
ENTRYPOINT ["echo"]
CMD ["Hello, World!"]
```
Running this container will output:
```
Hello, World!
```
You can override `CMD` by passing different arguments:
```bash
docker run myimage "This is a test"
```
Output:
```
This is a test
```

---

### **Overriding CMD at Runtime**

The `CMD` command can be overridden by specifying a different command when starting a container:

```bash
docker run myimage <override-command>
```

For example:
```bash
docker run mynginx echo "Nginx server is running!"
```
This will override the default `nginx` command and run `echo "Nginx server is running!"` instead.

---

### **Best Practices for Using `CMD`:**

1. **Use the Exec Form (`[]`)**:
   - Always prefer the exec form to avoid issues related to signal handling and process termination.
     ```dockerfile
     CMD ["python", "app.py"]
     ```

2. **Single `CMD` per Dockerfile**:
   - Ensure that your Dockerfile has only one `CMD` instruction. If multiple are defined, only the last one is used.

3. **Leverage `CMD` for Default Behavior**:
   - Use `CMD` for the default behavior that can be easily overridden by the user.

4. **Combine with `ENTRYPOINT` for Flexibility**:
   - Combine `CMD` with `ENTRYPOINT` for more flexible container configurations:
     ```dockerfile
     ENTRYPOINT ["python"]
     CMD ["app.py"]
     ```

---
