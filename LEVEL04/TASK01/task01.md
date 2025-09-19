## **🌟 Task 1 - Fix Dockerfile Issues on App Server 1**

**📌 Task Description**

The Nautilus DevOps team is working to create new images per requirements shared by the development team. One of the team members is working to create a Dockerfile on **App Server 1** in Stratos DC. While working on it, she ran into issues where the docker build is failing and displaying errors. Your task is to look into the issue and fix it to build an image as per the details mentioned below:

**Requirements:**
a. The Dockerfile is placed on **App Server 1** under `/opt/docker` directory.
b. Fix the issues with this file and ensure it can build the image.
c. Do not change the base image, any other valid configuration within the Dockerfile, or any of the data being used (e.g., `index.html`).

**Note**: Once you click the FINISH button, all existing containers will be destroyed, and a new image will be built from your Dockerfile.

👉 **Your task**: Fix the Dockerfile to ensure a successful image build while maintaining all valid configurations.


**Note**: There might be some other potential issues in your Dockerfile.I encountered these during setup, resolved it, and uploaded the fix so others can benefit if they face the same problem.


---

## 🔹 Step 1: Connect to App Server 1

```bash
ssh tony@stapp01
```

**Purpose**: Access the server where the Dockerfile is located.

---

## 🔹 Step 2: Switch to Root User

```bash
sudo su -
```

**Purpose**: Gain root privileges to perform Docker-related operations.

---

## 🔹 Step 3: Navigate to Dockerfile Directory

```bash
cd /opt/docker
```

**Purpose**: Move to the directory containing the Dockerfile and related files.

---

## 🔹 Step 4: Inspect the Dockerfile

```bash
cat Dockerfile
```

**Current Dockerfile**:
```dockerfile
FROM httpd:2.4.43

RUN sed -i "s/Listen 80/Listen 8080/g" /usr/local/apache2/conf/httpd.conf

RUN sed -i '/LoadModule\ ssl_module modules\/mod_ssl.so/s/^#//g' conf/httpd.conf

RUN sed -i '/LoadModule\ socache_shmcb_module modules\/mod_socache_shmcb.so/s/^#//g' conf/httpd.conf

RUN sed -i '/Include\ conf\/extra\/httpd-ssl.conf/s/^#//g' conf/httpd.conf

COPY /server.crt /usr/local/apache2/conf/server.crt

COPY /server.key /usr/local/apache2/conf/server.key

COPY ./index.html /usr/local/apache2/htdocs/
```

**Purpose**: Review the Dockerfile to identify issues causing the build failure.

---

## 🔹 Step 5: Identify the Problem

**Issue**: The `COPY` commands are failing because the file paths are incorrect:
- The Dockerfile assumes `server.crt` and `server.key` are in the root of the build context (`/`) or file system, but they are located in the `certs/` subdirectory.
- The `index.html` file is incorrectly referenced as `./index.html` but is located in the `html/` subdirectory.

**Error Output**:
```
ERROR: failed to build: failed to solve: failed to compute cache key: failed to calculate checksum of ref ...: "/index.html": not found
```

**Solution**: Update the `COPY` commands to use the correct relative paths within the build context.

---

## 🔹 Step 6: Fix the Dockerfile

Edit the Dockerfile to correct the file paths:

```bash
vi Dockerfile
```

**Updated Dockerfile**:
```dockerfile
FROM httpd:2.4.43

RUN sed -i "s/Listen 80/Listen 8080/g" /usr/local/apache2/conf/httpd.conf
RUN sed -i '/LoadModule\ ssl_module modules\/mod_ssl.so/s/^#//g' conf/httpd.conf
RUN sed -i '/LoadModule\ socache_shmcb_module modules\/mod_socache_shmcb.so/s/^#//g' conf/httpd.conf
RUN sed -i '/Include\ conf\/extra\/httpd-ssl.conf/s/^#//g' conf/httpd.conf

# Correct paths to files in the subdirectories
COPY ./certs/server.crt /usr/local/apache2/conf/server.crt
COPY ./certs/server.key /usr/local/apache2/conf/server.key
COPY ./html/index.html /usr/local/apache2/htdocs/index.html
```

**Changes Made**:
- Changed `COPY /server.crt` to `COPY ./certs/server.crt`
- Changed `COPY /server.key` to `COPY ./certs/server.key`
- Changed `COPY ./index.html` to `COPY ./html/index.html`

**Purpose**: Ensure the `COPY` commands reference the correct file locations relative to the build context.

---

## 🔹 Step 7: Build the Docker Image

```bash
docker build -t httpd_apache .
```

**Purpose**: Build the Docker image using the corrected Dockerfile.

**Expected Output**:
```
[+] Building ... (12/12) FINISHED
 => [1/8] FROM docker.io/library/httpd:2.4.43
 => [2/8] RUN sed -i "s/Listen 80/Listen 8080/g" /usr/local/apache2/conf/httpd.conf
 => [3/8] RUN sed -i '/LoadModule\ ssl_module modules\/mod_ssl.so/s/^#//g' conf/httpd.conf
 => [4/8] RUN sed -i '/LoadModule\ socache_shmcb_module modules\/mod_socache_shmcb.so/s/^#//g' conf/httpd.conf
 => [5/8] RUN sed -i '/Include\ conf\/extra\/httpd-ssl.conf/s/^#//g' conf/httpd.conf
 => [6/8] COPY ./certs/server.crt /usr/local/apache2/conf/server.crt
 => [7/8] COPY ./certs/server.key /usr/local/apache2/conf/server.key
 => [8/8] COPY ./html/index.html /usr/local/apache2/htdocs/index.html
 => exporting to image
 => => exporting layers
 => => writing image sha256:b4edb949230a...
 => => naming to docker.io/library/httpd_apache
```

---

## 🔹 Step 8: Verify the Image

```bash
docker images
```

**Expected Output**:
```
REPOSITORY     TAG       IMAGE ID       CREATED          SIZE
httpd_apache   latest    b4edb949230a   17 seconds ago   166MB
```

**Verification Points**:
- **REPOSITORY**: `httpd_apache`
- **TAG**: `latest`
- **SIZE**: Approximately 166MB

**Purpose**: Confirm the image was built successfully.

---

## 📋 Quick Commands for Fixing Dockerfile

```bash
ssh tony@stapp01

sudo su -

cd /opt/docker

vi Dockerfile

# Update with corrected COPY commands

docker build -t httpd_apache .

docker images
```
