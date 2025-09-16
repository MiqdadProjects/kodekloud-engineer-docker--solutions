## **🌟 Task 4 - Configure Apache in Container**

**📌 Task Description**

One of the **Nautilus DevOps team members** was working to configure services on a **kkloud container** running on **App Server 3**. Due to personal work, he is on PTO but we need to **finish his pending work ASAP**.

**Requirements:**
a. Install **apache2** in **kkloud** container using **apt** 
b. Configure Apache to listen on **port 8084** instead of default HTTP port
c. **Do not bind** to specific IP or hostname (listen on all interfaces)
d. Make sure **Apache service** is up and running inside the container
e. Keep the **container in running state**

👉 **Your task:** Complete in-container service configuration and customization.

---

## 🔹 Step 1: Connect to App Server 3

```bash
ssh banner@stapp03
sudo su -
```

**Purpose**: Access server with administrative privileges for container operations.

---

## 🔹 Step 2: Verify Container Status

```bash
docker ps
```

**Purpose**: Confirm kkloud container is running and accessible.

**Expected Output**:
```
CONTAINER ID   IMAGE     COMMAND     CREATED   STATUS   PORTS   NAMES
jkl012mno345   ubuntu    "/bin/bash"   1 day ago   Up 1 day           kkloud
```

---

## 🔹 Step 3: Enter Container Shell

```bash
docker exec -it kkloud /bin/bash
```

**Purpose**: Access interactive shell inside the running container.

**Shell Access Confirmed**: Prompt should change to container environment.

---

## 🔹 Step 4: Update Package Repository

```bash
apt update
```

**Purpose**: Update package lists to ensure latest software availability.

**Expected Output**:
```
Get:1 http://archive.ubuntu.com/ubuntu focal InRelease [265 kB]
...
Reading package lists... Done
```

---

## 🔹 Step 5: Install Apache2

```bash
apt install apache2 -y
```

**Purpose**: Install Apache web server with automatic yes confirmation.

**Installation Process**:
```
Reading package lists... Done
Building dependency tree       
...
Setting up apache2 (2.4.41-4ubuntu3) ...
```

**Package Components**:
- **apache2**: Main web server package
- **apache2-utils**: Additional utilities
- **apache2-bin**: Binary files and modules

---

## 🔹 Step 6: Install Text Editor (if needed)

```bash
apt install vim -y
```

**Purpose**: Install vim text editor for configuration file editing.

**Alternative editors**:
- **nano**: `apt install nano -y`
- **emacs**: `apt install emacs -y`

---

## 🔹 Step 7: Navigate to Apache Configuration

```bash
cd /etc/apache2
ls -la
```

**Purpose**: Access Apache configuration directory and view available files.

**Expected Output**:
```
drwxr-xr-x 8 root root 4096 Mar 15 10:30 .
drwxr-xr-x 1 root root 4096 Mar 15 10:30 ..
-rw-r--r-- 1 root root 7224 Oct 14  2019 apache2.conf
drwxr-xr-x 2 root root 4096 Mar 15 10:30 conf-available
drwxr-xr-x 2 root root 4096 Mar 15 10:30 conf-enabled
-rw-r--r-- 1 root root 1782 Oct 14  2019 envvars
drwxr-xr-x 2 root root 4096 Oct 14  2019 mods-available
drwxr-xr-x 2 root root 4096 Mar 15 10:30 mods-enabled
-rw-r--r-- 1 root root  320 Oct 14  2019 ports.conf
drwxr-xr-x 2 root root 4096 Mar 15 10:30 sites-available
drwxr-xr-x 2 root root 4096 Mar 15 10:30 sites-enabled
```

---

## 🔹 Step 8: Edit Ports Configuration

```bash
vi ports.conf
```

**Purpose**: Modify Apache port configuration to listen on port 8084.

**Configuration Change Required**:

**Find this line**:
```apache
Listen 80
```

**Change to**:
```apache
Listen 8084
```

**Vim Commands**:
1. Press `i` to enter insert mode
2. Navigate to the line and make changes
3. Press `Esc` to exit insert mode
4. Type `:wq` and press `Enter` to save and quit

---

## 🔹 Step 9: Verify Configuration Change

```bash
cat ports.conf
```

**Purpose**: Confirm the port change was saved correctly.

**Expected Output**:
```apache
# If you just change the port or add more ports here, you will likely also
# have to change the VirtualHost statement in
# /etc/apache2/sites-enabled/000-default.conf

Listen 8084

<IfModule ssl_module>
	Listen 443
</IfModule>

<IfModule mod_gnutls.c>
	Listen 443
</IfModule>
```

---

## 🔹 Step 10: Start Apache Service

```bash
service apache2 start
```

**Purpose**: Start the Apache web server with new configuration.

**Expected Output**:
```
 * Starting Apache httpd web server apache2                    [ OK ]
```

**Alternative command**:
```bash
systemctl start apache2
```

---

## 🔹 Step 11: Check Apache Service Status

```bash
service apache2 status
```

**Purpose**: Verify Apache is running successfully on the new port.

**Expected Output**:
```
 * apache2 is running
```

**Detailed status check**:
```bash
systemctl status apache2
```

---

## 🔹 Step 12: Verify Apache is Listening on Port 8084

```bash
netstat -tulnp | grep :8084
```

**Purpose**: Confirm Apache is listening on the correct port.

**Expected Output**:
```
tcp6       0      0 :::8084                 :::*                    LISTEN      1234/apache2
```

**Alternative command**:
```bash
ss -tulnp | grep :8084
```

---

## 🔹 Step 13: Test Apache Functionality

```bash
curl http://localhost:8084/
```

**Purpose**: Test Apache web server responds on the configured port.

**Expected Output**: Default Apache welcome page HTML content.

**Alternative test**:
```bash
wget -qO- http://localhost:8084/
```

---

## 🔹 Step 14: Exit Container (Keep Running)

```bash
exit
```

**Purpose**: Exit container shell while keeping Apache service and container running.

**Important**: Container remains running with Apache service active.

---

## 🔹 Step 15: Verify Container Still Running

```bash
docker ps
```

**Purpose**: Confirm kkloud container is still running after configuration.

**Expected Output**:
```
CONTAINER ID   IMAGE     COMMAND     CREATED   STATUS   PORTS   NAMES
jkl012mno345   ubuntu    "/bin/bash"   1 day ago   Up 1 day           kkloud
```

---

## 📋 Quick Apache Configuration Commands

```bash
# Complete Apache setup in container
ssh banner@stapp03

sudo su -

docker exec -it kkloud /bin/bash

apt update

apt install apache2 -y

apt install vim -y

cd /etc/apache2

vi ports.conf  # Change Listen 80 to Listen 8084

service apache2 start

service apache2 status

netstat -tulnp | grep :8084

curl http://localhost:8084/

exit

docker ps
```

---