## **🌟 Task 5 - Create Dockerfile**

**📌 Task Description**

As per recent requirements shared by the **Nautilus application development team**, they need **custom images** created for one of their projects. Several initial testing requirements have been shared with the DevOps team.

**Requirements:**
a. Create docker file **/opt/docker/Dockerfile** on **App Server 2** (keep **D** capital)
b. Use **ubuntu** as the base image
c. Install **apache2** and configure it to work on **port 5000**
d. **Do not update** any other Apache configuration settings like document root etc.

👉 **Your task:** Create a Dockerfile to build custom Apache image with specific port configuration.

---

## 🔹 Step 1: Connect to App Server 2

```bash
ssh steve@stapp02
```

**Purpose**: Access the server where Dockerfile needs to be created.

---

## 🔹 Step 2: Navigate to Target Directory

```bash
cd /opt/docker/
```

**Purpose**: Access the directory where Dockerfile must be created.

**If directory doesn't exist**:
```bash
sudo mkdir -p /opt/docker/
cd /opt/docker/
```

---

## 🔹 Step 3: Create Dockerfile

```bash
vi Dockerfile
```

**Purpose**: Create the Dockerfile with required specifications.

**Dockerfile Content**:

```dockerfile
FROM ubuntu

RUN apt-get update && apt-get install apache2 -y

RUN sed -i 's/Listen 80/Listen 5000/g' /etc/apache2/ports.conf

EXPOSE 5000

CMD ["apache2ctl", "-D", "FOREGROUND"]
```

**Dockerfile Breakdown**:
- **FROM ubuntu**: Use Ubuntu as base image
- **RUN apt-get update**: Update package lists
- **RUN apt-get install apache2 -y**: Install Apache with auto-confirmation
- **RUN sed -i**: Use sed to replace port 80 with 5000 in ports.conf
- **EXPOSE 5000**: Document that container listens on port 5000
- **CMD**: Start Apache in foreground mode to keep container running

---

## 🔹 Step 4: Alternative Dockerfile (with separate files)

**If you prefer using separate configuration files**:

```dockerfile
FROM ubuntu

RUN apt-get update

RUN apt-get install apache2 -y

ADD ports.conf /etc/apache2/ports.conf

ADD conf.conf /etc/apache2/sites-enabled/000-default.conf

EXPOSE 5000

CMD ["apache2ctl", "-D", "FOREGROUND"]
```

**This approach requires creating separate config files first**:

```bash
# Create ports.conf
cat > ports.conf << EOF
Listen 5000

<IfModule ssl_module>
    Listen 443
</IfModule>

<IfModule mod_gnutls.c>
    Listen 443
</IfModule>
EOF

# Create conf.conf
cat > conf.conf << EOF
<VirtualHost *:5000>
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/html
    ErrorLog \${APACHE_LOG_DIR}/error.log
    CustomLog \${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
EOF
```

---

## 🔹 Step 5: Verify Dockerfile Content

```bash
cat Dockerfile
```

**Purpose**: Review the Dockerfile content to ensure correctness.

**Expected Output**:
```dockerfile
FROM ubuntu

RUN apt-get update && apt-get install apache2 -y

RUN sed -i 's/Listen 80/Listen 5000/g' /etc/apache2/ports.conf

EXPOSE 5000

CMD ["apache2ctl", "-D", "FOREGROUND"]
```

---

## 🔹 Step 6: Test Build the Docker Image (Optional)

```bash
docker build -t custom-apache:test .
```

**Purpose**: Test if Dockerfile builds successfully.

**Expected Output**:
```
Sending build context to Docker daemon  2.048kB
Step 1/5 : FROM ubuntu
...
Successfully built abc123def456
Successfully tagged custom-apache:test
```

---

## 🔹 Step 7: Test Run Container (Optional)

```bash
docker run -d -p 5000:5000 --name apache-test custom-apache:test
```

**Purpose**: Test if the built image runs correctly.

**Test the service**:
```bash
curl http://localhost:5000/
```

**Cleanup test container**:
```bash
docker stop apache-test
docker rm apache-test
docker rmi custom-apache:test
```

---

## 📋 Quick Dockerfile Creation Commands

```bash
# Complete Dockerfile creation
ssh steve@stapp02
cd /opt/docker/
vi Dockerfile

# Add the dockerfile content:
# FROM ubuntu
# RUN apt-get update && apt-get install apache2 -y
# RUN sed -i 's/Listen 80/Listen 5000/g' /etc/apache2/ports.conf
# EXPOSE 5000
# CMD ["apache2ctl", "-D", "FOREGROUND"]

cat Dockerfile  # Verify content

# Optional: Test build
docker build -t custom-apache:test .
docker run -d -p 5000:5000 --name apache-test custom-apache:test
curl http://localhost:5000/
docker stop apache-test && docker rm apache-test && docker rmi custom-apache:test
```

---

## 🔧 Dockerfile Best Practices Applied

### **1. Efficient Layer Management**
```dockerfile
# Combine commands to reduce layers
RUN apt-get update && apt-get install apache2 -y
```

### **2. Port Configuration**
```dockerfile
# Use sed for inline configuration changes
RUN sed -i 's/Listen 80/Listen 5000/g' /etc/apache2/ports.conf
```

### **3. Container Lifecycle Management**
```dockerfile
# Run Apache in foreground to keep container alive
CMD ["apache2ctl", "-D", "FOREGROUND"]
```

### **4. Security and Maintenance**
```dockerfile
# Document exposed ports
EXPOSE 5000
```

---