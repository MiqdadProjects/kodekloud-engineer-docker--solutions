## **🌟 Task 2 - Fix Docker Compose Issues on App Server 2**

**📌 Task Description**

The Nautilus DevOps team is working to deploy an application on **App Server 2** in Stratos DC. Due to a misconfiguration in the docker-compose file, the deployment is failing. Your task is to identify and fix the issues to ensure the deployment works fine.

**Requirements:**
a. The `docker-compose.yml` file is present on **App Server 2** under `/opt/docker` directory.
b. Try to run the compose file and ensure it works fine.
c. Do not change the container names or update/alter any other valid configuration settings in the compose file or relevant data that could cause app failure.

**Note**: Once you click the FINISH button, all existing running/stopped containers will be destroyed, and your compose file will be run.

👉 **Your task**: Fix the docker-compose file to ensure a successful deployment.

**Note**: There might be some other potential issues in your Docker compose file . I encountered these during setup, resolved it, and uploaded the fix so others can benefit if they face the same problem.



---

## 🔹 Step 1: Connect to App Server 2

```bash
ssh steve@stapp02
```

**Purpose**: Access the server where the docker-compose file is located.

---

## 🔹 Step 2: Navigate to Compose File Directory

```bash
cd /opt/docker
```

**Purpose**: Move to the directory containing the `docker-compose.yml` file.

---

## 🔹 Step 3: Inspect the Docker Compose File

```bash
cat docker-compose.yml
```

**Current docker-compose.yml**:
```yaml
name: myapp

services:
    web:
        image: ./app
        container_name: python
        port:
            - "5000:5000"
        volumes:
            - ./app:/code
        depends_on:
            - redis
    redis_app:
        image: redis
        container_name: redis
```

**Purpose**: Review the compose file to identify misconfigurations.Remember to note that their could be any other issues with your docker compose file .

---

## 🔹 Step 4: Identify the Issues

**Issues**:
1. The `image: ./app` key is incorrect. To build an image from a local Dockerfile, the `build` key should be used instead of `image`.
2. The `port:` key is incorrectly singular; it should be `ports:` (plural).
3. The `depends_on: - redis` references a non-existent service name. The correct service name is `redis_app`.

**Solution**: Update the compose file to fix these issues while preserving valid configurations.

---

## 🔹 Step 5: Fix the Docker Compose File

Edit the compose file:

```bash
vi docker-compose.yml
```

**Corrected docker-compose.yml**:
```yaml
name: myapp

services:
  web:
    build: ./app
    container_name: python
    ports:
      - "5000:5000"
    volumes:
      - ./app:/code
    depends_on:
      - redis_app
  redis_app:
    image: redis
    container_name: redis
```

**Changes Made**:
- Changed `image: ./app` to `build: ./app` to build the image from the local Dockerfile.
- Changed `port:` to `ports:` to use the correct key.
- Changed `depends_on: - redis` to `depends_on: - redis_app` to reference the correct service name.

**Purpose**: Ensure the compose file uses correct syntax and references.

---

## 🔹 Step 6: Deploy the Compose Stack

```bash
docker compose up -d
```

**Purpose**: Start the containers in detached mode using the corrected compose file.

**Expected Output**:
```
✔ Network myapp_default  Created
✔ Container redis        Started
✔ Container python       Started
```

---

## 🔹 Step 7: Verify Running Containers

```bash
docker ps
```

**Expected Output**:
```
CONTAINER ID   IMAGE       COMMAND                  CREATED          STATUS          PORTS                    NAMES
15c0fe835d24   myapp-web   "/bin/sh -c 'python …"   43 seconds ago   Up 37 seconds   0.0.0.0:5000->5000/tcp   python
a31e346d9bc6   redis       "docker-entrypoint.s…"   45 seconds ago   Up 37 seconds   6379/tcp                 redis
```

**Verification Points**:
- **NAMES**: `python` and `redis`
- **PORTS**: `0.0.0.0:5000->5000/tcp` for the web service
- **STATUS**: `Up`

**Purpose**: Confirm both containers are running correctly.

---

## 🔹 Step 8: Test Application Functionality

```bash
curl http://localhost:5000
```

**Expected Output**:
```
This Compose/Flask demo has been viewed b'1' time(s).
```

**Purpose**: Verify the application is accessible and functioning as expected.

---

## 📋 Quick Commands for Fixing Docker Compose

```bash
ssh steve@stapp02

cd /opt/docker

vi docker-compose.yml

# Update with corrected build, ports, and depends_on

docker compose up -d

docker ps

curl http://localhost:5000
```
