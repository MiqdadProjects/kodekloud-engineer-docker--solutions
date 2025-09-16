## **🌟 Task 2 - Add User to Docker Group**

**📌 Task Description**

One of the **Nautilus project developers** needs access to run **docker commands** on **App Server 3**. This user is already created on the server but cannot run Docker commands without sudo.

**Requirements:**
a. User **kareem** is not able to run docker commands on **App Server 3**
b. Make required changes so this user can run docker commands **without sudo**

👉 **Your task:** Configure user permissions for Docker daemon access without root privileges.

---

## 🔹 Step 1: Connect to App Server 3

```bash
ssh banner@stapp03
```

**Purpose**: Access the server where user permissions need to be configured.

---

## 🔹 Step 2: Verify Current User Status

```bash
# Check if user exists
id kareem

# Check current groups
groups kareem
```

**Expected Output**:
```
uid=1001(kareem) gid=1001(kareem) groups=1001(kareem)
```

**Purpose**: Confirm user exists and check current group memberships.

---

## 🔹 Step 3: Verify Docker Group Exists

```bash
getent group docker
```

**Expected Output**:
```
docker:x:992:
```

**Purpose**: Confirm Docker group exists (created during Docker installation).

---

## 🔹 Step 4: Add User to Docker Group

```bash
sudo usermod -aG docker kareem
```

**Purpose**: Add kareem user to the docker group for daemon access.

**Command Breakdown**:
- **usermod**: User modification command
- **-a**: Append to group (don't remove from other groups)
- **-G**: Specify supplementary groups
- **docker**: Target group name
- **kareem**: Username to modify

---

## 🔹 Step 5: Verify Group Addition

```bash
groups kareem
```

**Expected Output**:
```
kareem : kareem docker
```

**Purpose**: Confirm user is now member of docker group.

---

## 🔹 Step 6: Test Docker Access (as kareem user)

```bash
# Switch to kareem user (if possible)
sudo su - kareem

# Test docker command
docker --version

# Test docker functionality
docker ps

# Return to original user
exit
```

**Expected Behavior**: Docker commands should work without sudo.

**Note**: User may need to log out and log back in for group changes to take effect.

---

## 📋 Quick User Permission Setup

```bash
# Complete user setup process
ssh banner@stapp03

id kareem

sudo usermod -aG docker kareem

groups kareem
# User needs to re-login for changes to take effect
```

---