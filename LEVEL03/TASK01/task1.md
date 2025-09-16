# 🐳 **Docker Level 3 Solutions**

**Expert-level Docker operations including networking, volume management, image transfer, and Docker Compose orchestration**

---

## **🌟 Task 1 - Create Docker Network**

**📌 Task Description**

The **Nautilus DevOps team** needs to set up several **docker environments** for different applications. One of the team members has been assigned a ticket to create some **docker networks** to be used later.

**Requirements:**
a. Create a docker network named **official** on **App Server 2** in **Stratos DC**
b. Configure it to use **macvlan** drivers
c. Set it to use subnet **192.168.30.0/24** and iprange **192.168.30.2/24**

👉 **Your task:** Master Docker networking with advanced macvlan driver configuration for container networking.

---

## 🔹 Step 1: Connect to App Server 2

```bash
ssh steve@stapp02
```

**Purpose**: Access the target server for network configuration.

---

## 🔹 Step 2: Check Existing Networks

```bash
docker network ls
```

**Purpose**: View current Docker networks before creating new one.

**Expected Output**:
```
NETWORK ID     NAME      DRIVER    SCOPE
abc123def456   bridge    bridge    local
ghi789jkl012   host      host      local
mno345pqr678   none      null      local
```

**Default Networks**:
- **bridge**: Default network for containers
- **host**: Uses host networking stack
- **none**: No networking

---

## 🔹 Step 3: Create Macvlan Network

```bash
docker network create official \
  --driver macvlan \
  --subnet 192.168.30.0/24 \
  --ip-range 192.168.30.2/24
```

**Purpose**: Create a macvlan network with specified subnet and IP range.

**Expected Output**:
```
stu901vwx234yz567abc890def123ghi456jkl789mno012pqr345stu678vwx901yz2
```

**Command Breakdown**:
- **docker network create**: Creates new Docker network
- **official**: Name of the network
- **--driver macvlan**: Uses macvlan driver for networking
- **--subnet 192.168.30.0/24**: Defines network subnet (256 IPs)
- **--ip-range 192.168.30.2/24**: Specifies assignable IP range

**Macvlan Driver Benefits**:
- **Direct network access**: Containers get direct access to physical network
- **MAC addresses**: Each container gets unique MAC address
- **Network isolation**: Better isolation compared to bridge networks
- **Performance**: Near-native network performance

---

## 🔹 Step 4: Verify Network Creation

```bash
docker network ls
```

**Expected Output**:
```
NETWORK ID     NAME      DRIVER    SCOPE
abc123def456   bridge    bridge    local
ghi789jkl012   host      host      local
mno345pqr678   none      null      local
stu901vwx234   official  macvlan   local
```

**Verification Points**:
- **NAME**: "official" network present
- **DRIVER**: Shows "macvlan"
- **SCOPE**: Shows "local"

---

## 🔹 Step 5: Inspect Network Details

```bash
docker network inspect official
```

**Purpose**: View detailed configuration of the created network.

**Expected Output Structure**:
```json
[
    {
        "Name": "official",
        "Id": "stu901vwx234...",
        "Created": "2024-03-15T10:30:00Z",
        "Scope": "local",
        "Driver": "macvlan",
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "192.168.30.0/24",
                    "IPRange": "192.168.30.2/24"
                }
            ]
        }
    }
]
```

**Key Configuration Elements**:
- **Subnet**: 192.168.30.0/24 (as specified)
- **IPRange**: 192.168.30.2/24 (as specified)
- **Driver**: macvlan (as specified)

---

## 🔹 Step 6: Test Network Functionality (Optional)

```bash
# Create test container using the network
docker run -d --name network-test --network official nginx:alpine

# Check container's network configuration
docker exec network-test ip addr show

# Cleanup test container
docker stop network-test
docker rm network-test
```

**Purpose**: Verify the network functions correctly with containers.

---

## 📋 Quick Network Creation Commands

```bash
# Complete network setup
ssh steve@stapp02

docker network ls

docker network create official \
  --driver macvlan \
  --subnet 192.168.30.0/24 \
  --ip-range 192.168.30.2/24

docker network ls

docker network inspect official
```

---