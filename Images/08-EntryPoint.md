### **3.10. ENTRYPOINT (Executable)**

The **`ENTRYPOINT`** directive in a Dockerfile specifies the command that will always run when the container starts. It's particularly useful for creating containers that are intended to be used as a specific application or service. Unlike `CMD`, which can be easily overridden when starting a container, `ENTRYPOINT` provides a more robust way to ensure that a particular command or script is executed.

---

#### **Purpose of `ENTRYPOINT`:**
- Defines the main executable for the container.
- Useful for containers designed to run a single, specific task (e.g., a web server, a script).
- Can be combined with `CMD` to set default arguments for the executable.

---

#### **Usage:**
```dockerfile
ENTRYPOINT ["executable", "param1", "param2"]
```

- This is the **exec form** (recommended), where the command is specified as a JSON array.
  - Example: `ENTRYPOINT ["python3", "app.py"]`

Alternatively, you can use the **shell form**:
```dockerfile
ENTRYPOINT command param1 param2
```
- Example: `ENTRYPOINT python3 app.py`
- Note: The shell form runs the command within a shell (`/bin/sh -c`), which can cause issues with signal handling. Therefore, the exec form is preferred.

---

### **Example: Using ENTRYPOINT for Container Initialization**

```dockerfile

FROM python:3.10-slim
WORKDIR /usr/src/app
COPY app.py .
RUN pip install --no-cache-dir flask
ENTRYPOINT ["python3", "app.py"]
```

---

---

### **Overriding ENTRYPOINT at Runtime**

While `ENTRYPOINT` is more rigid than `CMD`, you can still override it at runtime using the `--entrypoint` option:

```bash
docker run --entrypoint "/bin/bash" myimage
```

This will start a Bash shell instead of the default Python application.

---

### **Using `ENTRYPOINT` and `CMD` Together**

Combining `ENTRYPOINT` with `CMD` is a common pattern. `ENTRYPOINT` defines the executable, while `CMD` provides default arguments that can be overridden:

```dockerfile
# Using ENTRYPOINT and CMD together
FROM ubuntu:20.04

ENTRYPOINT ["echo"]
CMD ["Hello, Docker!"]
```

Running the container without arguments:
```bash
docker run myimage
```
Output:
```
Hello, Docker!
```

Overriding the `CMD` arguments:
```bash
docker run myimage "Welcome to Docker"
```
Output:
```
Welcome to Docker
```

---

### **Best Practices for Using `ENTRYPOINT`:**

1. **Use the Exec Form**:
   - Always prefer the exec form for better performance and proper signal handling.
     ```dockerfile
     ENTRYPOINT ["nginx", "-g", "daemon off;"]
     ```

2. **Make Containers Flexible**:
   - Use a script as your entrypoint to handle additional setup tasks, environment variables, or configuration:
     ```dockerfile
     ENTRYPOINT ["/usr/local/bin/entrypoint.sh"]
     ```

3. **Combine with `CMD` for Arguments**:
   - Leverage `CMD` to set default arguments that users can override.

---
