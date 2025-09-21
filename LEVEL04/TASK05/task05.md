## **🌟 Task 5 - Dockerize and Deploy Python App on App Server 2**

**📌 Task Description**

The Nautilus DevOps team needs to Dockerize a Python application and deploy it on **App Server 2** in Stratos DC. The application dependencies are defined in a `requirements.txt` file located under the `/python_app/src/` directory on App Server 2. Your task is to create a Dockerfile, build the image, and deploy a container to run the application.

**Requirements:**
1. Create a `Dockerfile` (case-sensitive) under the `/python_app` directory with the following specifications:
   - Use any Python image as the base image.
   - Install dependencies using the `requirements.txt` file.
   - Expose port `8084`.
   - Run the `server.py` script using the `CMD` instruction.
2. Build the Docker image and name it `nautilus/python-app`.
3. Run a container named `pythonapp_nautilus` using the built image.
4. Map the container's port `8084` to the host's port `8092`.
5. Test the application using the curl command: `curl http://localhost:8092`.

**Note**: Ensure all steps are followed precisely, and verify the deployment for successful application access.Also in your case ports can be different , so follow according to your task.

---

## 🔹 Step 1: Connect to App Server 2

```bash
ssh steve@stapp02
sudo su -
```

**Purpose**: Access App Server 2 and switch to the root user to perform the required tasks.

---

## 🔹 Step 2: Navigate to the Application Directory

```bash
cd /python_app
```

**Purpose**: Move to the `/python_app` directory where the application files are located.

---

## 🔹 Step 3: Create the Dockerfile

```bash
vi Dockerfile
```

**Dockerfile Content**:
```dockerfile
# Use the official Python slim image as the base image for a lightweight runtime environment
FROM python:3.14-rc-slim

# Set the working directory inside the container to /usr/src/python_app
WORKDIR /usr/src/python_app

# Copy requirements.txt from the src directory to the working directory
COPY ./src/requirements.txt ./

# Install the application dependencies defined in requirements.txt
RUN pip install -r requirements.txt

# Copy all files from the src directory to the working directory
COPY ./src .

# Expose port 8084 to allow external access to the application
EXPOSE 8084

# Specify the command to run the application using server.py
CMD ["python", "server.py"]
```

**Purpose**: Create a `Dockerfile` (case-sensitive) that:
- Uses `python:3.14-rc-slim` as the base image to provide a lightweight Python runtime environment.
- Sets the working directory to `/usr/src/python_app` for organizing application files.
- Copies `requirements.txt` from the `src` directory and installs dependencies using `pip install`.
- Copies the remaining application files (e.g., `server.py`) from the `src` directory.
- Exposes port `8084` to allow network traffic to the application.
- Specifies `server.py` as the entry point using the `CMD` instruction to start the Python application.

**Save the File**: Press `Esc`, then type `:wq` and press `Enter` to save and exit.

---

## 🔹 Step 4: Build the Docker Image

```bash
docker build -t nautilus/python-app .
```

**Purpose**: Build the Docker image from the `Dockerfile` and tag it as `nautilus/python-app`.

**Expected Output**:
```
Sending build context to Docker daemon  4.096kB
Step 1/7 : FROM python:3.14-rc-slim
...
Successfully built 6f6108091c7d
Successfully tagged nautilus/python-app:latest
```

---

## 🔹 Step 5: Verify the Built Image

```bash
docker images
```

**Expected Output**:
```
REPOSITORY            TAG       IMAGE ID       CREATED          SIZE
nautilus/python-app   latest    6f6108091c7d   42 seconds ago   135MB
```

**Purpose**: Confirm the `nautilus/python-app` image was created successfully.

---

## 🔹 Step 6: Run the Container

```bash
docker run -d --name pythonapp_nautilus -p 8092:8084 nautilus/python-app
```

**Purpose**: Start a container named `pythonapp_nautilus` in detached mode, mapping host port `8092` to container port `8084`.

---

## 🔹 Step 7: Verify Running Container

```bash
docker ps
```

**Expected Output**:
```
CONTAINER ID   IMAGE                 COMMAND              CREATED          STATUS         PORTS                    NAMES
10bc67528377   nautilus/python-app   "python server.py"   11 seconds ago   Up 8 seconds   0.0.0.0:8092->8084/tcp   pythonapp_nautilus
```

**Verification Points**:
- **NAMES**: `pythonapp_nautilus`
- **PORTS**: `0.0.0.0:8092->8084/tcp`
- **STATUS**: `Up`

**Purpose**: Confirm the container is running with the correct name and port mapping.

---

## 🔹 Step 8: Test Application Functionality

```bash
curl http://localhost:8092
```

**Expected Output**:
```
Welcome to xFusionCorp Industries!
```

**Purpose**: Verify the Python application is accessible and responding correctly.

---

## 🔹 Step 9: Additional Verification (Optional)

To further verify the container setup, you can inspect the container's contents:

```bash
docker exec -it pythonapp_nautilus sh
ls
```

**Expected Output**:
```
requirements.txt  server.py
```

Check the `requirements.txt` or `server.py` inside the container (if needed):

```bash
cat server.py
```

**Purpose**: Confirm the application files are correctly copied into the container.

---

## 📋 Quick Commands for Dockerizing and Deploying Python App

```bash
ssh steve@stapp02

sudo su -

cd /python_app

vi Dockerfile

# Add the following content to Dockerfile:
# Use the official Python slim image as the base image for a lightweight runtime environment
FROM python:3.14-rc-slim
# Set the working directory inside the container to /usr/src/python_app
WORKDIR /usr/src/python_app
# Copy requirements.txt from the src directory to the working directory
COPY ./src/requirements.txt ./
# Install the application dependencies defined in requirements.txt
RUN pip install -r requirements.txt
# Copy all files from the src directory to the working directory
COPY ./src .
# Expose port 8084 to allow external access to the application
EXPOSE 8084
# Specify the command to run the application using server.py
CMD ["python", "server.py"]

# Save and exit
docker build -t nautilus/python-app .

docker images

docker run -d --name pythonapp_nautilus -p 8092:8084 nautilus/python-app

docker ps

curl http://localhost:8092
```

**Purpose**: Summarize the commands for quick reference and execution.