## **🌟 Task 3 - Nginx Container with Port Mapping**

**📌 Task Description**

The **Nautilus DevOps team** is planning to host an application on a **nginx-based container**. There are a number of tickets already created for similar tasks. One ticket has been assigned to set up a **nginx container** on **Application Server 2**.

**Requirements:**
a. Pull **nginx:alpine-perl** docker image on **Application Server 2**
b. Create a container named **games** using the image you pulled
c. Map host port **3002** to container port **80**
d. Keep the **container in running state**

👉 **Your task:** Master Docker port mapping for web service accessibility and nginx variant usage.

---

## 🔹 Step 1: Connect to Application Server 2

```bash
ssh steve@stapp02
```

**Purpose**: Access the target server for nginx container deployment.

---

## 🔹 Step 2: Pull Nginx Alpine Perl Image

```bash
docker pull nginx:alpine-perl
```

**Purpose**: Download the nginx image with Alpine Linux and Perl support.

**Expected Output**:
```
alpine-perl: Pulling from library/nginx
7264a8db6415: Pull complete
...
Status: Downloaded newer image for nginx:alpine-perl
docker.io/library/nginx:alpine-perl
```

**Image Characteristics**:
- **Alpine Linux**: Minimal, security-focused distribution
- **Perl support**: Includes Perl modules for nginx
- **Size**: Smaller than regular nginx image
- **Security**: Reduced attack surface

---

## 🔹 Step 3: Verify Image Download

```bash
docker images nginx
```

**Expected Output**:
```
REPOSITORY   TAG           IMAGE ID       CREATED       SIZE
nginx        alpine-perl   abc890def123   2 weeks ago   23.4MB
```

**Purpose**: Confirm the image was downloaded with correct tag.

---

## 🔹 Step 4: Check Port Availability (Optional)

```bash
ss -tulnp | grep :3002
```

**Purpose**: Verify port 3002 is available on the host.

**Expected Result**: No output (port is free) or error message if port is in use.

---

## 🔹 Step 5: Create Container with Port Mapping

```bash
docker run -p 3002:80 -d --name games nginx:alpine-perl
```

**Purpose**: Create and run nginx container with port mapping from host to container.

**Command Breakdown**:
- **docker run**: Creates and starts new container
- **-p 3002:80**: Maps host port 3002 to container port 80
- **-d**: Runs in detached mode (background)
- **--name games**: Assigns name "games" to container
- **nginx:alpine-perl**: Uses the pulled image with Alpine and Perl

**Port Mapping Details**:
- **Host port 3002**: External access point
- **Container port 80**: Nginx default HTTP port
- **Protocol**: TCP (default for port mapping)
- **Accessibility**: Service available at localhost:3002

---

## 🔹 Step 6: Verify Container is Running

```bash
docker ps
```

**Expected Output**:
```
CONTAINER ID   IMAGE               COMMAND                  CREATED         STATUS         PORTS                  NAMES
ghi456jkl789   nginx:alpine-perl   "/docker-entrypoint.…"   1 minute ago    Up 1 minute    0.0.0.0:3002->80/tcp   games
```

**Verification Points**:
- **STATUS**: Should show "Up X seconds/minutes"
- **PORTS**: Should show "0.0.0.0:3002->80/tcp"
- **NAMES**: Should display "games"
- **IMAGE**: Should show "nginx:alpine-perl"

---

## 🔹 Step 7: Test Web Service Accessibility

```bash
curl localhost:3002
```

**Purpose**: Test that nginx web server is accessible on the mapped port.

**Expected Output**: Default nginx welcome page HTML content:
```html
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
...
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working...</p>
...
</html>
```

---

## 🔹 Step 8: Check Response Headers

```bash
curl -I localhost:3002
```

**Purpose**: Verify HTTP headers and response status.

**Expected Output**:
```
HTTP/1.1 200 OK
Server: nginx/1.21.6
Date: Wed, 15 Mar 2023 10:30:00 GMT
Content-Type: text/html
Content-Length: 615
Last-Modified: Tue, 25 Jan 2022 15:03:52 GMT
Connection: keep-alive
ETag: "61f01158-267"
Accept-Ranges: bytes
```

---

## 🔹 Step 9: Test from External Access (if applicable)

```bash
# Get server IP address
ip addr show | grep "inet " | grep -v 127.0.0.1

# Test from another server or local machine
curl http://[SERVER_IP]:3002/
```

**Purpose**: Verify external accessibility of the containerized service.

---

## 🔹 Step 10: Monitor Container Logs

```bash
docker logs games
```

**Purpose**: Check nginx access and error logs for the container.

**Expected Output**:
```
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
...
2023/03/15 10:30:00 [notice] 1#1: nginx/1.21.6
2023/03/15 10:30:00 [notice] 1#1: built by gcc 10.3.1 20211027 (Alpine 10.3.1_git20211027)
...
2023/03/15 10:30:00 [notice] 1#1: start worker processes
```

---

## 📋 Quick Port Mapping Setup

```bash
# Complete nginx with port mapping
ssh steve@stapp02

docker pull nginx:alpine-perl

docker run -p 3002:80 -d --name games nginx:alpine-perl

docker ps

curl http://localhost:3002

curl -I localhost:3002
```

---