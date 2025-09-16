## **🌟 Task 5 - Fix Static Website Container**

**📌 Task Description**

There is a **static website** running within a container named **nautilus** on **App Server 1**. Suddenly, issues started with the static website. The configuration details are:

**Requirements:**
a. Container's volume **/usr/local/apache2/htdocs** is mapped with host volume **/var/www/html**
b. Website should run on **host port 8080**
c. Command **curl http://localhost:8080/** should work on **App Server 1**

👉 **Your task:** Troubleshoot and fix the website container to restore functionality.

---

## 🔹 Step 1: Connect to App Server 1

```bash
ssh tony@stapp01
```

**Purpose**: Access the server hosting the problematic website container.

---

## 🔹 Step 2: Check Container Status

```bash
docker ps
```

**Purpose**: Determine current status of the nautilus container.

**Possible Scenarios**:
- **Container running**: Check if accessible
- **Container stopped**: Need to start it
- **Container missing**: Need to recreate it

---

## 🔹 Step 3: Check All Containers (Including Stopped)

```bash
docker ps -a
```

**Purpose**: Find the nautilus container even if it's currently stopped.

**Expected Output**:
```
CONTAINER ID   IMAGE     COMMAND              CREATED   STATUS                     NAMES
ghi789jkl012   httpd     "httpd-foreground"   2 hours ago   Exited (0) 1 hour ago    nautilus
```

---

## 🔹 Step 4: Start the Stopped Container

```bash
docker start nautilus
```

**Purpose**: Restart the existing container if it was stopped.

**Process Details**:
- **Preserves configuration**: Maintains original volume mounts and port mappings
- **Restores state**: Returns container to previous running configuration
- **Quick recovery**: Faster than recreating container

---

## 🔹 Step 5: Test Website Accessibility

```bash
curl http://localhost:8080/
```

**Purpose**: Verify website is accessible after starting the container.

**Expected Output**: HTML content from the website.

---

## 🔹 Step 6: Alternative - Recreate Container (If Starting Fails)

**If the container start fails or website still doesn't work:**

```bash
# Remove problematic container
docker rm nautilus

# Recreate with proper configuration
docker run -d -it --name nautilus \
  --mount type=bind,source=/var/www/html,target=/usr/local/apache2/htdocs \
  -p 8080:80 httpd
```

**Recreate Command Breakdown**:
- **docker run -d**: Create and run container in detached mode
- **-it**: Interactive terminal (maintains container running)
- **--name nautilus**: Assign specific container name
- **--mount type=bind**: Create bind mount between host and container
- **source=/var/www/html**: Host directory containing website files
- **target=/usr/local/apache2/htdocs**: Apache document root in container
- **-p 8080:80**: Map host port 8080 to container port 80
- **httpd**: Apache HTTP Server Docker image

---

## 🔹 Step 7: Final Website Test

```bash
curl http://localhost:8080/
```

**Purpose**: Confirm website is fully functional after fix.

**Additional Tests**:
```bash
# Check response headers
curl -I http://localhost:8080/

# Check container logs
docker logs nautilus

# Verify container is running
docker ps
```

---

## 📋 Quick Troubleshooting Commands

```bash
# Primary fix approach
ssh tony@stapp01
docker ps -a
docker start nautilus
curl http://localhost:8080/

# Alternative recreation approach
docker rm nautilus
docker run -d -it --name nautilus \
  --mount type=bind,source=/var/www/html,target=/usr/local/apache2/htdocs \
  -p 8080:80 httpd
curl http://localhost:8080/
```

---

## 🔧 Troubleshooting Common Issues

### **Issue 1: Container won't start**
**Symptoms**: Error messages when running `docker start`
**Solution**: Check container logs and recreate if necessary
```bash
docker logs nautilus
docker rm nautilus
# Use recreate commands from Step 6
```

### **Issue 2: Port 8080 already in use**
**Symptoms**: Port binding errors
**Solution**: Find and stop conflicting process
```bash
ss -tulnp | grep :8080
# Kill conflicting process or use different port
```

### **Issue 3: Website files missing**
**Symptoms**: 404 errors or empty responses
**Solution**: Verify host directory contains website files
```bash
ls -la /var/www/html/
# Ensure index.html or other web files exist
```

### **Issue 4: Permission issues**
**Symptoms**: 403 Forbidden errors
**Solution**: Check and fix file permissions
```bash
chmod -R 755 /var/www/html/
chown -R apache:apache /var/www/html/
```

---