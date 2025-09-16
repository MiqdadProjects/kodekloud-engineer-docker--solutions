## **🌟 Task 5 - Docker Compose Configuration**

**📌 Task Description**

The **Nautilus application development team** shared **static website content** that needs to be hosted on the **httpd web server** using a **containerized platform**. The team has shared details with the DevOps team, and we need to set up an environment according to those guidelines.

**Requirements:**
a. On **App Server 2** create a container named **httpd** using a **docker compose file** `/opt/docker/docker-compose.yml`
b. Use **httpd** (preferably latest tag) image for container and make sure container is named as **httpd**
c. Map **80** number port of container with port **6300** of docker host
d. Map container's **/usr/local/apache2/htdocs** volume with **/opt/dba** volume of docker host
e. **Do not modify** any data within these locations

👉 **Your task:** Master Docker Compose orchestration for web service deployment with volume and port mapping.

---

## 🔹 Step 1: Connect to App Server 2

```bash
ssh steve@stapp02
```

**Purpose**: Access the target server for Docker Compose configuration.

---

## 🔹 Step 2: Navigate to Working Directory

```bash
cd /opt/docker/
```

**Purpose**: Access the directory where docker-compose.yml must be created.

**If directory doesn't exist**:
```bash
sudo mkdir -p /opt/docker/
cd /opt/docker/
```

---

## 🔹 Step 3: Verify Host Volume Directory

```bash
ls -la /opt/dba/
```

**Purpose**: Confirm the host directory exists for volume mapping.

**Expected Result**: Directory should exist with web content files.

---

## 🔹 Step 4: Create Docker Compose File

```bash
vi docker-compose.yml
```

**Purpose**: Create the Docker Compose configuration file with specified requirements.

**Docker Compose Content**:

```yaml
version: '3.8'

services:
  httpd-service:
    container_name: httpd
    image: httpd:latest
    ports:
      - "6300:80"
    volumes:
      - /opt/dba:/usr/local/apache2/htdocs
```

**Configuration Breakdown**:
- **version**: Docker Compose file format version
- **services**: Defines container services
- **httpd-service**: Service name (can be any name)
- **container_name**: Specific container name "httpd"
- **image**: Uses httpd:latest Docker image
- **ports**: Maps host port 6300 to container port 80
- **volumes**: Mounts host directory to Apache document root

---

## 🔹 Step 5: Verify Compose File Content

```bash
cat docker-compose.yml
```

**Purpose**: Review the compose file content for accuracy.

**Expected Output**:
```yaml
version: '3.8'

services:
  httpd-service:
    container_name: httpd
    image: httpd:latest
    ports:
      - "6300:80"
    volumes:
      - /opt/dba:/usr/local/apache2/htdocs
```

---

## 🔹 Step 6: Validate Compose File Syntax

```bash
docker-compose config
```

**Purpose**: Validate Docker Compose file syntax and configuration.

**Expected Output**: Clean YAML output without errors indicates valid syntax.

---

## 🔹 Step 7: Start Services with Docker Compose

```bash
docker-compose up -d
```

**Purpose**: Start the httpd container using Docker Compose in detached mode.

**Expected Output**:
```
Creating network "docker_default" with the default driver
Creating httpd ... done
```

**Command Details**:
- **up**: Starts services defined in compose file
- **-d**: Runs containers in detached mode (background)
- **Creates**: Network, volumes, and containers as needed

---

## 🔹 Step 8: Verify Container Status

```bash
docker ps
```

**Expected Output**:
```
CONTAINER ID   IMAGE          COMMAND              CREATED         STATUS         PORTS                  NAMES
pqr678stu901   httpd:latest   "httpd-foreground"   1 minute ago    Up 1 minute    0.0.0.0:6300->80/tcp   httpd
```

**Verification Points**:
- **NAMES**: Should show "httpd"
- **IMAGE**: Should show "httpd:latest"
- **PORTS**: Should show "0.0.0.0:6300->80/tcp"
- **STATUS**: Should show "Up"

---

## 🔹 Step 9: Test Web Service

```bash
curl http://localhost:6300/
```

**Purpose**: Test Apache web server accessibility on mapped port.

**Expected Output**: HTML content from files in /opt/dba directory.

---

## 🔹 Step 10: Verify Volume Mounting

```bash
docker exec httpd ls -la /usr/local/apache2/htdocs/
```

**Purpose**: Confirm files from host directory are accessible in container.

**Expected Output**: Files from /opt/dba should be visible in container.

---

## 🔹 Step 11: Check Docker Compose Services

```bash
docker-compose ps
```

**Purpose**: View status of services managed by Docker Compose.

**Expected Output**:
```
Name    Command               State           Ports
httpd   httpd-foreground      Up      0.0.0.0:6300->80/tcp
```

---

## 📋 Quick Docker Compose Setup

```bash
# Complete Docker Compose setup
ssh steve@stapp02
cd /opt/docker/
ls -la /opt/dba/

# Create docker-compose.yml
vi docker-compose.yml
# Add the YAML content shown above

# Deploy and test
docker-compose config
docker-compose up -d
docker ps
curl http://localhost:6300/
docker-compose ps
```

---

## 🔧 Docker Compose Management Commands

### **Service Management**
```bash
# Start services
docker-compose up -d

# Stop services
docker-compose down

# Restart services
docker-compose restart

# View service logs
docker-compose logs httpd-service

# Scale services (if needed)
docker-compose up --scale httpd-service=2
```

### **Troubleshooting**
```bash
# View service status
docker-compose ps

# Check configuration
docker-compose config

# View logs
docker-compose logs

# Force recreate containers
docker-compose up --force-recreate -d
```

---
