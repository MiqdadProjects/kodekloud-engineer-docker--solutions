## **🌟 Task 3 - Delete Container**

**📌 Task Description**

One of the **Nautilus project developers** created a container on **App Server 2**. This container was created for **testing only** and now needs to be **deleted**.

**Requirements:**
a. Delete a container named **kke-container**
b. Container is running on **App Server 2** in **Stratos DC**

👉 **Your task:** Safely remove the specified test container from the server.

---

## 🔹 Step 1: Connect to App Server 2

```bash
ssh steve@stapp02
```

**Purpose**: Connect to App Server 2 where the target container is located.

---

## 🔹 Step 2: List Running Containers

```bash
docker ps
```

**Purpose**: Verify the container exists and check its current status.

**Expected Output**:
```
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS   PORTS   NAMES
xyz789abc123   ubuntu    "/bin/bash"   1 hour ago   Up 1 hour           kke-container
```

---

## 🔹 Step 3: Stop the Container

```bash
docker stop kke-container
```

**Purpose**: Gracefully stop the running container before removal.

**Process Details**:
- **Graceful shutdown**: Sends SIGTERM signal first
- **Timeout**: Waits 10 seconds before forcing SIGKILL
- **Data preservation**: Allows processes to clean up properly

---

## 🔹 Step 4: Remove the Container

```bash
docker rm kke-container
```

**Purpose**: Permanently delete the stopped container from the system.

**Removal Details**:
- **Container data**: Removes container filesystem and metadata
- **Network connections**: Cleans up network associations
- **Volume mounts**: Unmounts but preserves named volumes

---

## 🔹 Step 5: Verify Deletion

```bash
docker ps -a
```

**Purpose**: Confirm the container has been completely removed.

**Expected Result**: Container named "kke-container" should not appear in the list.

---

## 📋 Quick Deletion Commands

```bash
# Complete container removal
ssh steve@stapp02
docker ps
docker stop kke-container
docker rm kke-container
docker ps -a
```

**Alternative (Force removal)**:
```bash
# Force remove running container (use with caution)
docker rm -f kke-container
```

---