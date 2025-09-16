## **🌟 Task 4 - Transfer Docker Image Between Servers**

**📌 Task Description**

One of the **DevOps team members** was working to create a new **custom docker image** on **App Server 1**. He is done with his changes and the image is saved on the same server with name **blog:datacenter**. Recently, a requirement has been raised by a team to use that image for testing, but the team wants to test the same on **App Server 3**.

**Requirements:**
a. On **App Server 1** save the image **blog:datacenter** in an **archive**
b. **Transfer** the image archive to **App Server 3**
c. **Load** that image archive on **App Server 3** with **same name and tag**
d. **Note**: Docker is already installed on both servers; however, if its service is down please make sure to start it

👉 **Your task:** Master Docker image portability through archive creation, transfer, and loading across servers.

---

## 🔹 Step 1: Connect to App Server 1

```bash
ssh tony@stapp01
```

**Purpose**: Access the server containing the source Docker image for transfer.

---

## 🔹 Step 2: Verify Docker Service Status

```bash
sudo systemctl status docker
```

**Purpose**: Ensure Docker service is running on source server.

**If service is stopped**:
```bash
sudo systemctl start docker
sudo systemctl enable docker
```

---

## 🔹 Step 3: Verify Source Image Exists

```bash
docker images | grep blog
```

**Purpose**: Confirm the blog:datacenter image exists on App Server 1.

**Expected Output**:
```
blog         datacenter   mno345pqr678   2 hours ago   450MB
```

---

## 🔹 Step 4: Save Image to Archive

```bash
docker save -o blog.tar blog:datacenter
```

**Purpose**: Export the Docker image to a tar archive file.

**Command Breakdown**:
- **docker save**: Exports Docker image to tar archive
- **-o blog.tar**: Output filename for the archive
- **blog:datacenter**: Source image name and tag

**Process Details**:
- **Layers**: Saves all image layers and metadata
- **Size**: Archive size matches image size
- **Format**: Standard tar format with Docker metadata

---

## 🔹 Step 5: Verify Archive Creation

```bash
ls -la blog.tar
```

**Expected Output**:
```
-rw------- 1 root root 471859200 Mar 15 10:30 blog.tar
```

**Purpose**: Confirm archive was created with appropriate size.

---

## 🔹 Step 6: Transfer Archive to App Server 3

```bash
scp blog.tar banner@stapp03:~/
```

**Purpose**: Securely copy the image archive to the destination server.

**Transfer Details**:
- **Source**: blog.tar on App Server 1
- **Destination**: Home directory on App Server 3
- **Protocol**: SSH/SCP for secure transfer
- **Authentication**: Uses SSH key or password

**Alternative with specific path**:
```bash
scp blog.tar banner@172.16.238.12:~/
```

---

## 🔹 Step 7: Connect to App Server 3

```bash
ssh banner@stapp03
```

**Purpose**: Access destination server to load the transferred image.

---

## 🔹 Step 8: Verify Archive Transfer

```bash
ls -la ~/blog.tar
```

**Purpose**: Confirm the archive was successfully transferred.

**Expected Output**:
```
-rw------- 1 banner banner 471859200 Mar 15 10:30 blog.tar
```

---

## 🔹 Step 9: Verify Docker Service on Destination

```bash
sudo systemctl status docker
```

**Purpose**: Ensure Docker service is running on destination server.

**If service is stopped**:
```bash
sudo systemctl start docker
sudo systemctl enable docker
```

---

## 🔹 Step 10: Load Image from Archive

```bash
docker load -i blog.tar
```

**Purpose**: Import the Docker image from the tar archive.

**Expected Output**:
```
Loaded image: blog:datacenter
```

**Command Breakdown**:
- **docker load**: Imports Docker image from tar archive
- **-i blog.tar**: Input archive filename
- **Preserves**: All layers, metadata, and tags

---

## 🔹 Step 11: Verify Image Loading

```bash
docker images | grep blog
```

**Expected Output**:
```
blog         datacenter   mno345pqr678   2 hours ago   450MB
```

**Verification Points**:
- **REPOSITORY**: "blog"
- **TAG**: "datacenter"
- **IMAGE ID**: Same as source server
- **SIZE**: Matches original image size

---

## 🔹 Step 12: Test Image Functionality

```bash
docker run --rm blog:datacenter echo "Image transfer successful!"
```

**Expected Output**:
```
Image transfer successful!
```

**Purpose**: Verify the transferred image functions correctly.

---

## 📋 Quick Image Transfer Commands

**On App Server 1**:
```bash
ssh tony@stapp01
docker images | grep blog
docker save -o blog.tar blog:datacenter
ls -la blog.tar
scp blog.tar banner@stapp03:~/
```

**On App Server 3**:
```bash
ssh banner@stapp03

ls -la ~/blog.tar

docker load -i blog.tar

docker images | grep blog

docker run --rm blog:datacenter echo "Success!"
```

---
