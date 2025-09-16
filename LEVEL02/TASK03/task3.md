## **🌟 Task 3 - Create Image from Container**

**📌 Task Description**

One of the **Nautilus developers** was working to test new changes on a container. He wants to keep a **backup of his changes** to the container. A new request has been raised to create a **new image from this container**.

**Requirements:**
a. Create an image **demo:devops** on **Application Server 3**
b. Create from container **ubuntu_latest** that is running on same server

👉 **Your task:** Master container-to-image conversion for preserving development changes.

---

## 🔹 Step 1: Connect to App Server 3

```bash
ssh banner@stapp03
```

**Purpose**: Access the server containing the source container.

---

## 🔹 Step 2: Verify Source Container Status

```bash
docker ps
```

**Purpose**: Confirm ubuntu_latest container is running.

**Expected Output**:
```
CONTAINER ID   IMAGE     COMMAND     CREATED   STATUS   PORTS   NAMES
def456ghi789   ubuntu    "/bin/bash"   2 hours ago   Up 2 hours           ubuntu_latest
```

**Container Requirements**:
- **NAME**: ubuntu_latest
- **STATUS**: Up (running) - though commit works on stopped containers too

---

## 🔹 Step 3: Inspect Container Changes (Optional)

```bash
docker diff ubuntu_latest
```

**Purpose**: View filesystem changes made in the container.

**Output Examples**:
```
C /etc
A /etc/apt/sources.list.d/custom.list
C /usr
A /usr/local/bin/custom-script.sh
```

**Change Types**:
- **A**: Added file/directory
- **D**: Deleted file/directory
- **C**: Changed file/directory

---

## 🔹 Step 4: Create Image from Container

```bash
docker commit ubuntu_latest demo:devops
```

**Purpose**: Create new image from container's current state with specified name and tag.

**Expected Output**:
```
sha256:ghi789jkl012mno345pqr678stu901vwx234yz567abc890def123ghi456jkl
```

**Command Breakdown**:
- **docker commit**: Creates image from container
- **ubuntu_latest**: Source container name
- **demo:devops**: Target image name and tag
- Returns SHA256 hash of new image

---

## 🔹 Step 5: Verify New Image Creation

```bash
docker images
```

**Expected Output**:
```
REPOSITORY   TAG      IMAGE ID       CREATED          SIZE
demo         devops   ghi789jkl012   30 seconds ago   72.8MB
ubuntu       latest   abc123def456   3 weeks ago      72.8MB
```

**Verification Points**:
- **REPOSITORY**: "demo"
- **TAG**: "devops" 
- **CREATED**: Recent timestamp
- **SIZE**: May be larger than base if changes were made

---

## 🔹 Step 6: Test New Image Functionality

```bash
# Run container from new image
docker run --rm -it demo:devops bash

# Inside container - test any custom changes
ls -la /usr/local/bin/
# Check for any custom files or configurations

# Exit container
exit
```

**Purpose**: Verify the committed image contains all container changes.

---

## 🔹 Step 7: Add Commit Message (Best Practice)

```bash
# Alternative commit with message
docker commit -m "Development changes for testing" ubuntu_latest demo:devops-v2
```

**Purpose**: Add descriptive message for better image management.

**Message Benefits**:
- **Documentation**: Explains what changes are included
- **Version tracking**: Helps identify different image versions
- **Collaboration**: Helps team understand image purpose

---

## 📋 Quick Image Creation Commands

```bash
# Complete image commit process
ssh banner@stapp03

docker ps

docker diff ubuntu_latest

docker commit ubuntu_latest demo:devops

docker images

docker run --rm -it demo:devops bash
```

---