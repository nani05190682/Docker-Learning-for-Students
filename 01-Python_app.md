 Here's a practical example to demonstrate the usage of the **`FROM`** directive:

---

### **Example: Building a Simple Python Application using a Base Image**

Suppose you have a Python application with the following structure:

```
my-python-app/
├── Dockerfile
├── app.py
└── requirements.txt
```

**`app.py`**:
```python
# A simple Python application
from flask import Flask

app = Flask(__name__)

@app.route('/')
def hello_world():
    return "Hello, Docker!"

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```

**`requirements.txt`**:
```
flask
```

---

### **Dockerfile**

```dockerfile
FROM python:3.10-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
EXPOSE 5000
CMD ["python", "app.py"]
```

---

### **Explanation of the Dockerfile:**

1. **Base Image**:
   ```dockerfile
   FROM python:3.10-slim
   ```
   - Uses the official **Python 3.10 slim** image, which is a lightweight version of Python. This helps reduce the overall image size.

2. **Set Working Directory**:
   ```dockerfile
   WORKDIR /app
   ```
   - Sets `/app` as the working directory inside the container. All subsequent commands will run in this directory.

3. **Copy and Install Dependencies**:
   ```dockerfile
   COPY requirements.txt .
   RUN pip install --no-cache-dir -r requirements.txt
   ```
   - Copies the `requirements.txt` file into the container.
   - Installs Python packages listed in `requirements.txt` using `pip`.

4. **Copy Application Files**:
   ```dockerfile
   COPY . .
   ```
   - Copies all files from the current directory on your host machine (`my-python-app/`) into the working directory (`/app`) inside the container.

5. **Expose Port**:
   ```dockerfile
   EXPOSE 5000
   ```
   - Informs Docker that the container will listen on port 5000 (used by Flask).

6. **Set Default Command**:
   ```dockerfile
   CMD ["python", "app.py"]
   ```
   - Specifies the command to run when the container starts: `python app.py`.

---

### **Building and Running the Docker Image**

1. **Build the Docker Image**:
   ```bash
   docker build -t my-python-app .
   ```
   - The `-t my-python-app` flag tags the image with the name `my-python-app`.

2. **Run the Docker Container**:
   ```bash
   docker run -d -p 5000:5000 my-python-app
   ```
   - The `-d` flag runs the container in detached mode.
   - The `-p 5000:5000` flag maps port 5000 on your local machine to port 5000 in the container.

3. **Test the Application**:
   - Open your browser and go to `http://localhost:5000`.
   - You should see the message: **"Hello, Docker!"**

---

### **Benefits of Using `FROM` with a Slim Image**:
- By using `python:3.10-slim` instead of the full `python:3.10`, you significantly reduce the image size.
- This results in faster builds, reduced storage usage, and quicker deployment times.

This example demonstrates how to use the **`FROM`** directive to select an appropriate base image for your application, ensuring a balance between functionality and efficiency.
