# 🐳 **Docker Level 1 Solutions**

**Complete guide for KodeKloud Engineer Docker Level 1 tasks with detailed explanations and troubleshooting**

---

## **🌟 Task 1 - Docker Installation and Setup**

**📌 Task Description**

The **Nautilus DevOps team** met with the application development team and decided to **containerize several applications**. The DevOps team wants to do some testing per the following requirements:

**Requirements:**
a. Install **docker-ce** and **docker-compose** packages on **App Server 3**
b. Start **docker service**
c. Verify installation and service status

👉 **Your task:** Install and configure Docker Community Edition with Docker Compose on the specified server.

---

## 🔹 Step 1: Connect to App Server 3

```bash
ssh banner@stapp03
sudo su -
```

**Purpose**: Connect to App Server 3 and switch to root for administrative privileges.

---

## 🔹 Step 2: Check OS Release

```bash
cat /etc/os-release
```

**Purpose**: Identify the operating system to use correct installation commands.

**Expected Output** (CentOS example):
```
NAME="CentOS Linux"
VERSION="8"
ID="centos"
```

---

## 🔹 Step 3: Install Docker Repository Prerequisites

```bash
sudo dnf -y install dnf-plugins-core
```

**Purpose**: Install core plugins for DNF package manager to manage repositories.

**Command Breakdown**:
- **dnf**: Package manager for RHEL-based systems
- **-y**: Automatically answer yes to prompts
- **dnf-plugins-core**: Essential plugins for repository management

---

## 🔹 Step 4: Add Docker Repository

```bash
sudo dnf config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```

**Purpose**: Add official Docker repository to system's package sources.

**Repository Details**:
- **Official Docker repo**: Ensures latest stable Docker releases
- **CentOS specific**: Uses CentOS-compatible packages
- **GPG signed**: Provides package integrity verification

---

## 🔹 Step 5: Install Docker Components

```bash
sudo dnf install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

**Purpose**: Install complete Docker ecosystem with all necessary components.

**Package Details**:
- **docker-ce**: Docker Community Edition engine
- **docker-ce-cli**: Command-line interface for Docker
- **containerd.io**: Container runtime used by Docker
- **docker-buildx-plugin**: Extended build capabilities
- **docker-compose-plugin**: Docker Compose functionality

---

## 🔹 Step 6: Enable and Start Docker Service

```bash
sudo systemctl enable --now docker
```

**Purpose**: Enable Docker to start automatically on boot and start it immediately.

**Service Management**:
- **enable**: Configure service to start on boot
- **--now**: Start service immediately
- **Combined command**: Efficient single-step activation

---

## 🔹 Step 7: Verify Docker Installation

```bash
docker --version
```

**Expected Output**:
```
Docker version 24.0.7, build afdd53b
```

**Purpose**: Confirm Docker engine is installed and accessible.

---

## 🔹 Step 8: Verify Docker Compose Installation

```bash
docker compose version
```

**Expected Output**:
```
Docker Compose version v2.21.0
```

**Purpose**: Confirm Docker Compose plugin is installed and functional.

---

## 🔹 Step 9: Check Docker Service Status

```bash
systemctl status docker
```

**Expected Output**:
```
● docker.service - Docker Application Container Engine
   Loaded: loaded (/usr/lib/systemd/system/docker.service; enabled; vendor preset: disabled)
   Active: active (running)
```

**Status**: ✅ **Docker service is running and enabled**

---

## 📋 Quick Installation Commands

```bash
# Complete installation sequence
ssh banner@stapp03
sudo su -
cat /etc/os-release
sudo dnf -y install dnf-plugins-core
sudo dnf config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
sudo dnf install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
sudo systemctl enable --now docker
docker --version
docker compose version
systemctl status docker
```

---