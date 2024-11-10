### **3.3. ENV (Environment Variables)**

The **`ENV`** directive in a Dockerfile is used to define environment variables within your Docker container. These environment variables can be used to customize your application's behavior, pass configuration settings, and make your Docker images more flexible and portable.

---

#### **Purpose of `ENV`:**
- Set key-value pairs as environment variables within the container.
- These variables can be accessed by applications and scripts running inside the container.
- Useful for setting default values, configurations, or secrets (though sensitive data should be managed with caution).

---

#### **Usage:**
```dockerfile
ENV <key>=<value>
```

- **`<key>`**: The name of the environment variable (e.g., `APP_ENV`, `DATABASE_URL`).
- **`<value>`**: The value assigned to the environment variable (e.g., `production`, `localhost:5432`).

---

### **Example 1: Setting a Single Environment Variable**

```dockerfile
# Step 1: Specify the base image
FROM python:3.10-slim
ENV APP_ENV=production
ENV DEBUG=false
COPY app.py /app/app.py
WORKDIR /app
CMD ["python", "app.py"]
```

In this example:
- `APP_ENV` is set to `production`, indicating the application is running in a production environment.
- `DEBUG` is set to `false` to disable debug mode.

---

### **Example 2: Setting Multiple Environment Variables in a Single Line**

```dockerfile

FROM node:18-alpine
ENV NODE_ENV=production PORT=3000 LOG_LEVEL=info
COPY . /usr/src/app
WORKDIR /usr/src/app
RUN npm install
EXPOSE 3000
CMD ["npm", "start"]
```

In this example:
- The `NODE_ENV` variable is used to define the environment as `production`.
- The `PORT` is set to `3000` for the application to listen on.
- The `LOG_LEVEL` is set to `info` to control the verbosity of application logs.

---

### **How to Access Environment Variables Inside a Container**

- **Python Example**:
  ```python
  import os
  app_env = os.getenv('APP_ENV')
  print(f"Running in {app_env} mode")
  ```

- **Node.js Example**:
  ```javascript
  // Accessing environment variables
  const port = process.env.PORT || 3000;
  console.log(`Server is running on port ${port}`);
  ```

---

### **Viewing Environment Variables in a Running Container**

To see the environment variables set in a running container, use:

```bash
docker exec <container_id> printenv
```

Or to inspect them before running the container:

```bash
docker run --rm --env-file <env_file> <image> printenv
```

---

### **Best Practices for Using `ENV`:**

1. **Avoid Hardcoding Sensitive Data**:
   - Don't use `ENV` to store sensitive information like passwords or API keys. Use Docker Secrets or environment files (`--env-file`) instead.

2. **Use Default Values**:
   - Set sensible default values for variables, but allow them to be overridden at runtime:
     ```dockerfile
     ENV PORT=3000
     ```

3. **Leverage Environment Files**:
   - Use `.env` files to load multiple environment variables:
     ```bash
     docker run --env-file .env myapp
     ```

4. **Keep it Clean**:
   - Avoid redundant environment variables to keep your image clean and maintainable.

---
