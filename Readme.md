# Jenkins CI/CD Pipeline for Node.js Application

![Jenkins](https://img.shields.io/badge/Jenkins-D24939?style=for-the-badge&logo=jenkins&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Node.js](https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=node.js&logoColor=white)

---

## 📋 Table of Contents

- [Overview](#-overview)
- [Architecture](#-architecture)
- [Prerequisites](#-prerequisites)
- [Quick Start](#-quick-start)
- [Jenkins Setup](#-jenkins-setup)
- [Pipeline Stages](#-pipeline-stages)
- [Configuration](#️-configuration)
- [Docker Commands](#-docker-commands)
- [Accessing the Application](#-accessing-the-application)
- [Troubleshooting](#-troubleshooting)
- [References](#-references)

---

## 🎯 Overview

This project demonstrates an **automated CI/CD pipeline** for a containerized Node.js application using **Jenkins** on Windows. The pipeline seamlessly integrates with GitHub to build, test, and deploy Docker containers with zero manual intervention.

### Key Features

✅ Automated code checkout from GitHub  
✅ Docker image building with version tagging  
✅ Automated testing integration  
✅ Container deployment with health checks  
✅ Port mapping and container lifecycle management  
✅ Deployment logging and monitoring

---

## 🏗️ Architecture

```
GitHub Repository
       ↓
Jenkins Pipeline
       ↓
   ┌─────────────────────────┐
   │   Pipeline Stages       │
   ├─────────────────────────┤
   │ 1. Checkout Code        │
   │ 2. Build Docker Image   │
   │ 3. Run Tests            │
   │ 4. Deploy Container     │
   │ 5. Verify Deployment    │
   └─────────────────────────┘
       ↓
Docker Container (Port 3100 → 3000)
       ↓
Node.js Application
```

---

## 🔧 Prerequisites

Ensure the following are installed and configured on your Windows system:

| Requirement            | Version       | Download Link                                              |
| ---------------------- | ------------- | ---------------------------------------------------------- |
| **Windows OS**         | 10/11 or WSL2 | -                                                          |
| **Docker Desktop**     | Latest        | [Download](https://www.docker.com/products/docker-desktop) |
| **Jenkins**            | 2.x or higher | [Download](https://www.jenkins.io/download/)               |
| **Git**                | Latest        | [Download](https://git-scm.com/downloads)                  |
| **Node.js** (optional) | 18+           | [Download](https://nodejs.org/)                            |

### System Requirements

- **RAM**: Minimum 4GB (8GB recommended)
- **Disk Space**: 10GB free space
- **Network**: Internet connection for pulling dependencies

### Verification Commands

```bash
# Verify Docker
docker --version
docker ps

# Verify Jenkins
# Access http://localhost:8080

# Verify Git
git --version
```

---

## 🚀 Quick Start

### 1. Clone the Repository

```bash
git clone https://github.com/rishi08083/Task2.git
cd Task2
```

### 2. Test Docker Build Locally

```bash
docker build -t task2-demo:latest .
docker run -d --name task2-container -p 3100:3000 task2-demo:latest
```

### 3. Access the Application

Open your browser and navigate to:

```
http://localhost:3100
```

---

## ⚙️ Jenkins Setup

### Step 1: Create New Jenkins Job

1. Open Jenkins Dashboard at `http://localhost:8080`
2. Click **New Item**
3. Enter job name (e.g., `Task2-CICD-Pipeline`)
4. Select **Pipeline** and click **OK**

### Step 2: Configure Source Code Management

1. Under **Pipeline** section, select **Pipeline script from SCM**
2. Choose **Git** as SCM
3. Repository URL:
   ```
   https://github.com/rishi08083/Task2.git
   ```
4. Branch: `*/main`
5. Script Path: `Jenkinsfile`

### Step 3: Configure Build Triggers (Optional)

- **Poll SCM**: `H/5 * * * *` (checks every 5 minutes)
- **GitHub webhook**: Configure webhook in GitHub repository settings

### Step 4: Grant Docker Permissions

Ensure Jenkins user has permissions to execute Docker commands:

```bash
# Add Jenkins user to docker-users group (Windows)
# Or run Jenkins as administrator
```

### Step 5: Save and Build

Click **Save** → **Build Now**

---

## 📊 Pipeline Stages

| Stage           | Description                                           | Commands                              | Duration |
| --------------- | ----------------------------------------------------- | ------------------------------------- | -------- |
| **🔍 Checkout** | Clones the latest code from GitHub repository         | `git checkout`                        | ~10s     |
| **🔨 Build**    | Builds Docker image from Dockerfile                   | `docker build -t task2-demo:latest .` | ~1-2min  |
| **✅ Test**     | Runs automated tests (dummy example included)         | `echo "All tests passed!"`            | ~5s      |
| **🚀 Deploy**   | Stops old container, starts new one with port mapping | `docker run -d -p 3100:3000 ...`      | ~15s     |
| **📋 Logs**     | Displays deployment logs and container status         | `docker logs`                         | ~5s      |

### Jenkinsfile Structure

```groovy
pipeline {
    agent any

    environment {
        IMAGE_NAME = "task2-demo"
        CONTAINER_NAME = "task2-container"
        PORT = "3100"
    }

    stages {
        stage('Checkout') { ... }
        stage('Build Docker Image') { ... }
        stage('Test') { ... }
        stage('Deploy') { ... }
    }

    post {
        success { ... }
        failure { ... }
    }
}
```

---

## ⚙️ Configuration

### Environment Variables

Configure these in your `Jenkinsfile`:

```groovy
environment {
    IMAGE_NAME = "task2-demo"           // Docker image name
    CONTAINER_NAME = "task2-container"  // Container name
    PORT = "3100"                       // Host port (maps to container:3000)
    DOCKER_REGISTRY = ""                // Optional: Docker registry URL
}
```

### Port Mapping

| Host Port | Container Port | Protocol | Purpose             |
| --------- | -------------- | -------- | ------------------- |
| 3100      | 3000           | TCP      | Node.js application |

**Note**: Ensure port `3100` is not used by another application on your host machine.

### Dockerfile Configuration

```dockerfile
FROM node:18-alpine
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD ["npm", "start"]
```

---

## 🐳 Docker Commands

### Manual Operations

#### Build Image

```bash
docker build -t task2-demo:latest .
```

#### Run Container

```bash
docker run -d --name task2-container -p 3100:3000 task2-demo:latest
```

#### Stop Container

```bash
docker stop task2-container
```

#### Remove Container

```bash
docker rm -f task2-container
```

#### View Logs

```bash
docker logs task2-container

# Follow logs in real-time
docker logs -f task2-container
```

#### Inspect Container

```bash
docker inspect task2-container
```

#### Check Running Containers

```bash
docker ps

# Show all containers (including stopped)
docker ps -a
```

#### Remove Image

```bash
docker rmi task2-demo:latest
```

---

## 🌐 Accessing the Application

### After Successful Deployment

Once the Jenkins pipeline completes successfully, access your application at:

```
http://localhost:3100
```

### Verify Deployment

```bash
# Check if container is running
docker ps | grep task2-container

# Test API endpoint (if applicable)
curl http://localhost:3100

# Check container health
docker inspect --format='{{.State.Health.Status}}' task2-container
```

### Network Access

- **Localhost**: `http://localhost:3100`
- **Local Network**: `http://<YOUR_IP>:3100`
- **Production**: Configure reverse proxy (Nginx/Apache)

---

## 🔧 Troubleshooting

### Common Issues and Solutions

#### Issue 1: Port Already in Use

**Error**: `Bind for 0.0.0.0:3100 failed: port is already allocated`

**Solution**:

```bash
# Find process using the port
netstat -ano | findstr :3100

# Kill the process (replace PID)
taskkill /PID <PID> /F

# Or change PORT in Jenkinsfile
```

#### Issue 2: Docker Permission Denied

**Error**: `Got permission denied while trying to connect to the Docker daemon`

**Solution**:

- Run Jenkins as Administrator
- Add Jenkins user to `docker-users` group
- Restart Jenkins service

#### Issue 3: Image Build Fails

**Error**: `npm install` fails during build

**Solution**:

```bash
# Clear Docker cache
docker builder prune -a

# Rebuild with no cache
docker build --no-cache -t task2-demo:latest .
```

#### Issue 4: Container Exits Immediately

**Solution**:

```bash
# Check logs for errors
docker logs task2-container

# Run container interactively for debugging
docker run -it --rm task2-demo:latest /bin/sh
```

#### Issue 5: Jenkins Cannot Find Jenkinsfile

**Solution**:

- Verify `Jenkinsfile` exists in repository root
- Check branch name (should be `main`)
- Ensure correct Script Path in Jenkins configuration

### Debug Commands

```bash
# View Docker daemon logs
docker events

# Check Docker system info
docker system info

# View Jenkins console output
# Jenkins Dashboard → Job → Build #X → Console Output
```

---

## 📚 References

### Documentation

- [Jenkins Pipeline Syntax](https://www.jenkins.io/doc/book/pipeline/syntax/)
- [Docker Documentation](https://docs.docker.com/)
- [Docker Compose Guide](https://docs.docker.com/compose/)
- [Node.js Best Practices](https://github.com/goldbergyoni/nodebestpractices)

### Additional Resources

- [Dockerfile Best Practices](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/)
- [Jenkins Docker Plugin](https://plugins.jenkins.io/docker-plugin/)
- [GitHub Webhooks Guide](https://docs.github.com/en/developers/webhooks-and-events/webhooks)

---

## 📝 Project Structure

```
Task2/
├── Jenkinsfile              # CI/CD pipeline definition
├── Dockerfile              # Docker image configuration
├── package.json            # Node.js dependencies
├── package-lock.json       # Locked dependency versions
├── README.md              # This file
└── app.js                 # Application source code
```

---

## 🎯 Future Enhancements

- [ ] Add unit and integration tests
- [ ] Implement health check endpoints
- [ ] Add Docker Compose for multi-container setup
- [ ] Configure environment-specific deployments (dev/staging/prod)
- [ ] Integrate SonarQube for code quality analysis
- [ ] Add Slack/Email notifications for build status
- [ ] Implement blue-green deployment strategy
- [ ] Add Kubernetes deployment manifests

---

## 👨‍💻 Author

**Rishi Manoj Shori**

- GitHub: [@rishi08083](https://github.com/rishi08083)
- Project: [Task2 Repository](https://github.com/rishi08083/Task2)

**Last Updated**: October 22, 2025  
**Version**: 1.0.0

---
