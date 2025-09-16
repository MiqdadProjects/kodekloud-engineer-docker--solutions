## **🌟 Task 2 - Container with Volume Mapping**

**📌 Task Description**

The **Nautilus DevOps team** is testing **applications containerization**, which is supposed to be migrated to **docker container-based environments** soon. In today's stand-up meeting, one team member was assigned a task to create and test a docker container with certain requirements.

**Requirements:**
a. On **App Server 3** in **Stratos DC** pull **nginx** image (preferably latest tag)
b. Create a new container with name **blog** from the image you just pulled
c. Map the host volume **/opt/security** with container volume **/usr/src/**
d. There is a **sample.txt** file present on same server under **/tmp**; copy that file to **/opt/security**
e. Keep the **container in running state**

👉 **Your task:** Master Docker volume mapping and file management between host and containers.

---

## 🔹 Step 1: Connect to App Server 3

```bash
ssh banner@stapp03
```

**Purpose**: Access the target server for container and volume operations.

---

## 🔹 Step 2: Pull Nginx Image

```bash
docker pull nginx
```

**Purpose**: Download the latest nginx image from Docker Hub.

**Expected Output**:
```
Using default tag: latest
latest: Pulling from library/nginx
a2abf6c4d29d: Pull complete
...
Status: Downloaded newer image for nginx:latest
docker.io/library/nginx:latest
```

**Image Details**:
- **Latest tag**: Most recent stable release
- **Size**: Typically around 130MB
- **Base**: Debian-based nginx image

---

## 🔹 Step 3: Verify Host Directory Exists

```bash
ls -la /opt/security/
```

**Purpose**: Check if host directory exists for volume mapping.

**If directory doesn't exist**:
```bash
sudo mkdir -p /opt/security/
```

---

## 🔹 Step 4: Copy Sample File to Host Directory

```bash
cp /tmp/sample.txt /opt/security/
```

**Purpose**: Copy the required file to the host directory that will be mounted.

**Verify file copy**:
```bash
ls -la /opt/security/
```

**Expected Output**:
```
total 12
drwxr-xr-x 2 root root 4096 Mar 15 10:30 .
drwxr-xr-x 3 root root 4096 Mar 15 10:30 ..
-rw-r--r-- 1 root root   23 Mar 15 10:30 sample.txt
```

---

## 🔹 Step 5: Create Container with Volume Mapping

```bash
docker run -d --name blog -v /opt/security:/usr/src/ nginx
```

**Purpose**: Create and run nginx container with volume mapping.

**Command Breakdown**:
- **docker run**: Creates and starts new container
- **-d**: Runs in detached mode (background)
- **--name blog**: Assigns name "blog" to container
- **-v /opt/security:/usr/src/**: Mounts host directory to container directory
- **nginx**: Uses pulled nginx image

**Volume Mapping Details**:
- **Host path**: /opt/security (contains sample.txt)
- **Container path**: /usr/src/ (where files will be accessible)
- **Mount type**: Bind mount (bidirectional sync)

---

## 🔹 Step 6: Verify Container is Running

```bash
docker ps
```

**Expected Output**:
```
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS     NAMES
vwx234yz567a   nginx     "/docker-entrypoint.…"   30 seconds ago  Up 29 seconds  80/tcp    blog
```

**Status Verification**:
- **STATUS**: Should show "Up X seconds/minutes"
- **NAMES**: Should display "blog"
- **IMAGE**: Should show "nginx"

---

## 🔹 Step 7: Verify File Accessibility in Container

```bash
docker exec -it blog sh
```

**Purpose**: Access container shell to verify volume mapping and file accessibility.

**Inside Container**:
```bash
cd /usr/src/
ls -l
```

**Expected Output**:
```
total 4
-rw-r--r-- 1 root root 23 Mar 15 10:30 sample.txt
```

**Verification Points**:
- **File exists**: sample.txt present in /usr/src/
- **File size**: Matches original file size
- **Permissions**: Should be readable

---

## 🔹 Step 8: Test File Content

```bash
# Inside container
cat sample.txt
```

**Purpose**: Verify file content is accessible and intact.

**Exit container**:
```bash
exit
```

---

## 🔹 Step 9: Test Bidirectional Sync (Optional)

```bash
# Add file from host side
echo "Host created file" > /opt/security/host-file.txt

# Check in container
docker exec blog ls -la /usr/src/

# Add file from container side
docker exec blog sh -c "echo 'Container created file' > /usr/src/container-file.txt"

# Check on host
ls -la /opt/security/
```

**Purpose**: Verify bidirectional synchronization between host and container.

---

## 📋 Quick Volume Mapping Commands

```bash
# Complete volume mapping setup
ssh banner@stapp03

docker pull nginx

cp /tmp/sample.txt /opt/security/

ls -la /opt/security/

docker run -d --name blog -v /opt/security:/usr/src/ nginx

docker ps

docker exec -it blog sh

cd /usr/src/ && ls -l && exit
```

---