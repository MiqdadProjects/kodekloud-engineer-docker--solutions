## **🌟 Task 4 - Dockerize and Deploy Node App on App Server 3**

**📌 Task Description**

The Nautilus DevOps team needs to Dockerize a Node.js application and deploy it on **App Server 3** in Stratos DC. The application files, including `package.json` (describing app dependencies) and `server.js` (defining the web app framework), are already placed under the `/node_app` directory on App Server 3. Your task is to create a Dockerfile, build the image, and deploy a container to run the application.

**Requirements:**
1. Create a `Dockerfile` (case-sensitive) under the `/node_app` directory with the following specifications:
   - Use any Node image as the base image.
   - Install dependencies using the `package.json` file.
   - Use `server.js` in the `CMD` instruction.
   - Expose port `8088`.
2. Build the Docker image and name it `nautilus/node-web-app`.
3. Run a container named `nodeapp_nautilus` using the built image.
4. Map the container's port `8088` to the host's port `8095`.
5. Test the application using the curl command: `curl http://localhost:8095`.

**Note**: Ensure all steps are followed precisely, and verify the deployment for successful application access.

---

## 🔹 Step 1: Connect to App Server 3

```bash
ssh banner@stapp03
sudo su -
```

**Purpose**: Access App Server 3 and switch to the root user to perform the required tasks.

---

## 🔹 Step 2: Navigate to the Application Directory

```bash
cd /node_app
```

**Purpose**: Move to the `/node_app` directory where the `package.json` and `server.js` files are located.

---

## 🔹 Step 3: Create the Dockerfile

```bash
vi Dockerfile
```

**Dockerfile Content**:
```dockerfile
# Use the official Node.js latest image as the base image
FROM node:latest

# Set the working directory inside the container to /app
WORKDIR /app

# Copy package.json to the working directory
COPY package.json ./

# Install the application dependencies defined in package.json
RUN npm install

# Copy all application files to the working directory
COPY . .

# Expose port 8088 to allow external access to the application
EXPOSE 8088

# Specify the command to run the application using server.js
CMD ["node", "server.js"]
```

**Purpose**: Create a `Dockerfile` (case-sensitive) that:
- Uses `node:latest` as the base image to provide the Node.js runtime environment.
- Sets the working directory to `/app` for organizing application files.
- Copies `package.json` and installs dependencies using `npm install` to set up the application.
- Copies the remaining application files (e.g., `server.js`) to the container.
- Exposes port `8088` to allow network traffic to the application.
- Specifies `server.js` as the entry point using the `CMD` instruction to start the Node.js application.

**Save the File**: Press `Esc`, then type `:wq` and press `Enter` to save and exit.

---

## 🔹 Step 4: Build the Docker Image

```bash
docker build -t nautilus/node-web-app .
```

**Purpose**: Build the Docker image from the `Dockerfile` and tag it as `nautilus/node-web-app`.

**Expected Output**:
```
Sending build context to Docker daemon  4.096kB
Step 1/7 : FROM node:latest
...
Successfully built ca087b842584
Successfully tagged nautilus/node-web-app:latest
```

---

## 🔹 Step 5: Verify the Built Image

```bash
docker images
```

**Expected Output**:
```
REPOSITORY              TAG       IMAGE ID       CREATED          SIZE
nautilus/node-web-app   latest    ca087b842584   25 seconds ago   1.15GB
```

**Purpose**: Confirm the `nautilus/node-web-app` image was created successfully.

---

## 🔹 Step 6: Run the Container

```bash
docker run -d --name nodeapp_nautilus -p 8095:8088 nautilus/node-web-app
```

**Purpose**: Start a container named `nodeapp_nautilus` in detached mode, mapping host port `8095` to container port `8088`.

---

## 🔹 Step 7: Verify Running Container

```bash
docker ps
```

**Expected Output**:
```
CONTAINER ID   IMAGE                   COMMAND                  CREATED          STATUS          PORTS                    NAMES
93d83689da57   nautilus/node-web-app   "docker-entrypoint.s…"   37 seconds ago   Up 10 seconds   0.0.0.0:8095->8088/tcp   nodeapp_nautilus
```

**Verification Points**:
- **NAMES**: `nodeapp_nautilus`
- **PORTS**: `0.0.0.0:8095->8088/tcp`
- **STATUS**: `Up`

**Purpose**: Confirm the container is running with the correct name and port mapping.

---

## 🔹 Step 8: Test Application Functionality

```bash
curl http://localhost:8095
```

**Expected Output**:
```
Welcome to xFusionCorp Industries!
```

**Purpose**: Verify the Node.js application is accessible and responding correctly.

---

## 🔹 Step 9: Additional Verification (Optional)

To further verify the container setup, you can inspect the container's contents:

```bash
docker exec -it nodeapp_nautilus sh
ls
```

**Expected Output**:
```
Dockerfile  node_modules  package-lock.json  package.json  server.js
```

Check the `Dockerfile` inside the container:

```bash
cat Dockerfile
```

**Expected Output**:
```
# Use the official Node.js latest image as the base image
FROM node:latest

# Set the working directory inside the container to /app
WORKDIR /app

# Copy package.json to the working directory
COPY package.json ./

# Install the application dependencies defined in package.json
RUN npm install

# Copy all application files to the working directory
COPY . .

# Expose port 8088 to allow external access to the application
EXPOSE 8088

# Specify the command to run the application using server.js
CMD ["node", "server.js"]
```

**Purpose**: Confirm the application files and `Dockerfile` are correctly copied into the container.

---

## 📋 Quick Commands for Dockerizing and Deploying Node App

```bash
ssh banner@stapp03

sudo su -

cd /node_app

vi Dockerfile

# Add the following content to Dockerfile:
# Use the official Node.js latest image as the base image
FROM node:latest
# Set the working directory inside the container to /app
WORKDIR /app
# Copy package.json to the working directory
COPY package.json ./
# Install the application dependencies defined in package.json
RUN npm install
# Copy all application files to the working directory
COPY . .
# Expose port 8088 to allow external access to the application
EXPOSE 8088
# Specify the command to run the application using server.js
CMD ["node", "server.js"]

# Save and exit
docker build -t nautilus/node-web-app .
docker images

docker run -d --name nodeapp_nautilus -p 8095:8088 nautilus/node-web-app

docker ps

curl http://localhost:8095
```

**Purpose**: Summarize the commands for quick reference and execution.