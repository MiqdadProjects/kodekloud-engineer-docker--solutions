## **🌟 Task 4 - Copy Files Between Host and Container**

**📌 Task Description**

The **Nautilus DevOps team** has conditional data on **App Server 2** in **Stratos Datacenter**. There is a container **ubuntu_latest** running on the same server. A request was received to **copy data** from the docker host to the container.

**Requirements:**
a. Copy encrypted file **/tmp/nautilus.txt.gpg** from **docker host**
b. Copy to **ubuntu_latest** container in **/opt/** location
c. **Do not modify** the file in any way

👉 **Your task:** Transfer files between host filesystem and container filesystem safely.

---

## 🔹 Step 1: Connect to App Server 2

```bash
ssh steve@stapp02
```

**Purpose**: Access the server containing both the source file and target container.

---

## 🔹 Step 2: Verify Source File Exists

```bash
cd /tmp
ls -la nautilus.txt.gpg
```

**Purpose**: Confirm the source file exists and check its properties.

**Expected Output**:
```
-rw-r--r-- 1 root root 1234 Mar 15 10:30 nautilus.txt.gpg
```

**File Details**:
- **.gpg extension**: Indicates GPG encrypted file
- **File permissions**: Standard read permissions
- **File size**: Varies based on content

---

## 🔹 Step 3: Verify Target Container Status

```bash
docker ps
```

**Purpose**: Confirm ubuntu_latest container is running and accessible.

**Expected Output**:
```
CONTAINER ID   IMAGE     COMMAND     CREATED   STATUS   PORTS   NAMES
def456ghi789   ubuntu    "/bin/bash"   1 day ago   Up 1 day           ubuntu_latest
```

**Container Requirements**:
- **STATUS**: Must be "Up" (running)
- **NAME**: Should be "ubuntu_latest"

---

## 🔹 Step 4: Copy File from Host to Container

```bash
docker cp /tmp/nautilus.txt.gpg ubuntu_latest:/opt/
```

**Purpose**: Transfer the encrypted file from host to container destination.

**Command Breakdown**:
- **docker cp**: Docker copy command for file transfer
- **/tmp/nautilus.txt.gpg**: Source path on host
- **ubuntu_latest**: Target container name
- **:/opt/**: Destination path inside container

---

## 🔹 Step 5: Verify File Copy Success

```bash
docker exec -it ubuntu_latest sh
```

**Purpose**: Access the container's shell to verify file transfer.

**Inside Container Shell**:
```bash
cd /opt/
ls -l
```

**Expected Output**:
```
total 4
-rw-r--r-- 1 root root 1234 Mar 15 10:30 nautilus.txt.gpg
```

**Verification Points**:
- **File exists**: nautilus.txt.gpg present in /opt/
- **File size**: Matches original file size
- **Permissions**: Preserved from source

---

## 🔹 Step 6: Exit Container Shell

```bash
exit
```

**Purpose**: Return to host shell after verification.

---

## 📋 Quick File Copy Commands

```bash
# Complete file transfer process
ssh steve@stapp02
ls -la /tmp/nautilus.txt.gpg
docker ps
docker cp /tmp/nautilus.txt.gpg ubuntu_latest:/opt/
docker exec -it ubuntu_latest sh
cd /opt/ && ls -l
exit
```

---