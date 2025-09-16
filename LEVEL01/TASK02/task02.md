## **🌟 Task 2 - Deploy Nginx Container**

**📌 Task Description**

**Nautilus DevOps team** is testing application deployments on application servers. They need to deploy a **nginx container** on **Application Server 3**.

**Requirements:**
a. Create a container named **nginx_3** using **nginx** image with **alpine** tag
b. Ensure container is in **running state**

👉 **Your task:** Deploy and verify nginx container with specific naming and image requirements.

---

## 🔹 Step 1: Connect to App Server 3

```bash
ssh banner@stapp03
```

**Purpose**: Connect to the target server for container deployment.

---

## 🔹 Step 2: Pull Nginx Alpine Image (Optional)

```bash
docker pull nginx:alpine
```

**Purpose**: Pre-download the nginx Alpine image (optional since `docker run` pulls automatically).

**Image Benefits**:
- **Alpine Linux**: Minimal, security-oriented Linux distribution
- **Smaller size**: Reduced attack surface and faster deployment
- **Latest updates**: Includes security patches and optimizations

---

## 🔹 Step 3: Create and Run Nginx Container

```bash
docker run -d --name nginx_3 nginx:alpine
```

**Purpose**: Create and start nginx container with specified requirements.

**Command Breakdown**:
- **docker run**: Creates and starts a new container
- **-d**: Runs container in detached mode (background)
- **--name nginx_3**: Assigns specific name to the container
- **nginx:alpine**: Uses nginx image with alpine tag

---

## 🔹 Step 4: Verify Container Status

```bash
docker ps
```

**Expected Output**:
```
CONTAINER ID   IMAGE          COMMAND                  CREATED         STATUS         PORTS     NAMES
abc123def456   nginx:alpine   "/docker-entrypoint.…"   2 minutes ago   Up 2 minutes   80/tcp    nginx_3
```

**Status Verification**:
- **STATUS**: Should show "Up X minutes"
- **NAMES**: Should display "nginx_3"
- **PORTS**: Should show "80/tcp" (nginx default port)

---

## 🔹 Step 5: Additional Container Information

```bash
docker inspect nginx_3
```

**Purpose**: Get detailed information about the container configuration.

---

## 📋 Quick Deployment Commands

```bash
# Complete nginx deployment
ssh banner@stapp03
docker run -d --name nginx_3 nginx:alpine
docker ps
```

---