## **🌟 Task 3 - Deploy Docker Compose Stack on App Server 3**

**📌 Task Description**

The Nautilus Application development team recently finished developing an app they want to deploy on a containerized platform. The team wants to test the deployment on **App Server 3** in Stratos DC before going live, using a docker-compose file to set up a complete containerized stack. Your task is to create and deploy the compose file as per the details below:

**Requirements:**
a. Create a docker-compose file `/opt/itadmin/docker-compose.yml` (exact name) on **App Server 3**.
b. The compose should deploy two services (web and DB) with the following details:

**Web Service**:
- Container name must be `php_host`.
- Use image `php` with any Apache tag.
- Map `php_host` container's port `80` to host port `8085`.
- Map `php_host` container's `/var/www/html` volume to host volume `/var/www/html`.

**DB Service**:
- Container name must be `mysql_host`.
- Use image `mariadb` with any tag (preferably `latest`).
- Map `mysql_host` container's port `3306` to host port `3306`.
- Map `mysql_host` container's `/var/lib/mysql` volume to host volume `/var/lib/mysql`.
- Set `MYSQL_DATABASE=database_host` and use a custom user (not `root`) with a complex password for DB connections.

c. After running `docker-compose up`, the app should be accessible via `curl <server-ip or hostname>:8085/`.

**Note**: Once you click the FINISH button, all currently running/stopped containers will be destroyed, and the stack will be deployed using your compose file.

👉 **Your task**: Create and deploy a docker-compose stack meeting all specified requirements.

---

## 🔹 Step 1: Connect to App Server 3

```bash
ssh banner@stapp03
```

**Purpose**: Access the server where the docker-compose file will be created.

---

## 🔹 Step 2: Switch to Root User

```bash
sudo su -
```

**Purpose**: Gain root privileges to perform Docker-related operations and create files in `/opt/itadmin`.

---

## 🔹 Step 3: Create Directory for Compose File

```bash
mkdir -p /opt/itadmin
cd /opt/itadmin
```

**Purpose**: Ensure the `/opt/itadmin` directory exists and navigate to it.

---

## 🔹 Step 4: Create Docker Compose File

```bash
vi docker-compose.yml
```

**docker-compose.yml**:
```yaml
version: '3.8'

services:
  web:
    container_name: php_host
    image: php:8.2-apache
    ports:
      - "8085:80"
    volumes:
      - /var/www/html:/var/www/html

  db:
    container_name: mysql_host
    image: mariadb:latest
    ports:
      - "3306:3306"
    volumes:
      - /var/lib/mysql:/var/lib/mysql
    environment:
      - MYSQL_DATABASE=database_host
      - MYSQL_USER=nautilus_user
      - MYSQL_PASSWORD=Str0ngP@ssw0rd!
      - MYSQL_ROOT_PASSWORD=another_strong_password
```

**File Details**:
- **version**: Specifies Docker Compose version `3.8` for compatibility.
- **web service**:
  - Uses `php:8.2-apache` as the image.
  - Maps port `8085` (host) to `80` (container).
  - Maps host `/var/www/html` to container `/var/www/html`.
- **db service**:
  - Uses `mariadb:latest` as the image.
  - Maps port `3306` (host) to `3306` (container).
  - Maps host `/var/lib/mysql` to container `/var/lib/mysql`.
  - Sets environment variables for database configuration, including a custom user `nautilus_user` with a complex password.

**Purpose**: Create a compose file that meets all specified requirements.

---

## 🔹 Step 5: Deploy the Compose Stack

```bash
docker compose up -d
```

**Purpose**: Start the containers in detached mode using the compose file.

**Expected Output**:
```
✔ Network itadmin_default  Created
✔ Container mysql_host     Started
✔ Container php_host       Started
```

---

## 🔹 Step 6: Verify Running Containers

```bash
docker ps
```

**Expected Output**:
```
CONTAINER ID   IMAGE            COMMAND                  CREATED              STATUS              PORTS                    NAMES
67a18197b68e   mariadb:latest   "docker-entrypoint.s…"   About a minute ago   Up About a minute   0.0.0.0:3306->3306/tcp   mysql_host
0de570bc7008   php:8.2-apache   "docker-php-entrypoi…"   About a minute ago   Up About a minute   0.0.0.0:8085->80/tcp     php_host
```

**Verification Points**:
- **NAMES**: `php_host` and `mysql_host`
- **PORTS**: `0.0.0.0:8085->80/tcp` for `php_host`, `0.0.0.0:3306->3306/tcp` for `mysql_host`
- **STATUS**: `Up`

**Purpose**: Confirm both containers are running with correct configurations.

---

## 🔹 Step 7: Test Application Accessibility

```bash
curl stapp03:8085
```

**Expected Output**:
```
<html>
    <head>
        <title>Welcome to xFusionCorp Industries!</title>
    </head>
    <body>
        Welcome to xFusionCorp Industries!
    </body>
</html>
```

**Purpose**: Verify the web application is accessible via the specified port.

---

## 📋 Quick Commands for Deploying Compose Stack

```bash
ssh banner@stapp03

sudo su -

mkdir -p /opt/itadmin

cd /opt/itadmin

vi docker-compose.yml

# Add the compose file content

docker compose up -d

docker ps

curl stapp03:8085
```