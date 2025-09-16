# 🐳 **Docker Level 2 Solutions**

**Advanced Docker operations including image management, user permissions, container commits, and Dockerfile creation**

---

## **🌟 Task 1 - Pull and Re-tag Docker Image**

**📌 Task Description**

**Nautilus project developers** are planning to start testing on a new project. As per their meeting with the DevOps team, they want to test **containerized environment application features**.

**Requirements:**
a. Pull **busybox:musl** image on **App Server 2** in **Stratos DC**
b. **Re-tag** (create new tag) this image as **busybox:local**

👉 **Your task:** Master Docker image management by pulling and retagging images for local development.

---

## 🔹 Step 1: Connect to App Server 2

```bash
ssh steve@stapp02
```

**Purpose**: Access App Server 2 for image management operations.

---

## 🔹 Step 2: Pull Busybox Musl Image

```bash
docker pull busybox:musl
```

**Purpose**: Download the busybox image with musl tag from Docker Hub.

**Expected Output**:
```
musl: Pulling from library/busybox
7b2699543f22: Pull complete
Digest: sha256:abc123def456...
Status: Downloaded newer image for busybox:musl
docker.io/library/busybox:musl
```

**Image Details**:
- **busybox**: Minimal Unix utilities in a single executable
- **musl tag**: Uses musl libc instead of glibc (smaller, lighter)
- **Size**: Typically under 1.5MB

---

## 🔹 Step 3: Verify Image Download

```bash
docker images busybox
```

**Expected Output**:
```
REPOSITORY   TAG    IMAGE ID       CREATED       SIZE
busybox      musl   abc123def456   2 weeks ago   1.41MB
```

**Purpose**: Confirm the image was successfully downloaded with correct tag.

---

## 🔹 Step 4: Create New Tag

```bash
docker tag busybox:musl busybox:local
```

**Purpose**: Create a new tag (alias) for the same image with a local identifier.

**Tag Benefits**:
- **Local identification**: Clearly marks image for local development
- **Version management**: Creates multiple references to same image
- **No storage overhead**: Tags are just pointers to same image layers

---

## 🔹 Step 5: Verify Both Tags Exist

```bash
docker images busybox
```

**Expected Output**:
```
REPOSITORY   TAG    IMAGE ID       CREATED       SIZE
busybox      local  abc123def456   2 weeks ago   1.41MB
busybox      musl   abc123def456   2 weeks ago   1.41MB
```

**Verification Points**:
- **Same IMAGE ID**: Both tags reference the same image
- **Different tags**: "musl" and "local" tags both present
- **Same size**: No additional storage used

---

## 🔹 Step 6: Test Tagged Image Functionality

```bash
docker run --rm busybox:local echo "Local tag working!"
```

**Expected Output**:
```
Local tag working!
```

**Purpose**: Verify the newly tagged image functions correctly.

---

## 📋 Quick Image Tagging Commands

```bash
# Complete image pull and tag process
ssh steve@stapp02

docker pull busybox:musl

docker tag busybox:musl busybox:local

docker images busybox

docker run --rm busybox:local echo "Test successful"
```

---