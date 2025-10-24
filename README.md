# 🌌 Solar System - Node.js Application

A beautiful and interactive web application showcasing the Solar System and its planets, built with HTML, Node.js, Express, and MongoDB. This project demonstrates modern DevOps practices with Docker containerization, Kubernetes deployment, and CI/CD automation using GitHub Actions.

[![Node.js Version](https://img.shields.io/badge/node-%3E%3D18.0.0-brightgreen)](https://nodejs.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

---

## 📋 Table of Contents

- [Features](#-features)
- [Prerequisites](#-prerequisites)
- [Installation](#-installation)
- [Running the Application](#-running-the-application)
- [Testing](#-testing)
- [Docker Deployment](#-docker-deployment)
- [Kubernetes Deployment](#-kubernetes-deployment)
- [CI/CD Pipeline](#-cicd-pipeline)
- [Project Structure](#-project-structure)
- [Environment Variables](#-environment-variables)
- [API Endpoints](#-api-endpoints)
- [Contributing](#-contributing)
- [License](#-license)

---

## ✨ Features

- 🪐 Interactive Solar System visualization
- 🌍 Detailed information about each planet
- 🗄️ MongoDB integration for planet data storage
- 🐳 Docker containerization support
- ☸️ Kubernetes-ready deployment manifests
- 🔄 Automated CI/CD pipeline with GitHub Actions
- ✅ Unit testing with Mocha and Chai
- 📊 Code coverage reporting with NYC
- 🔍 Health check endpoints (liveness and readiness probes)

---

## 📦 Prerequisites

Before you begin, ensure you have the following installed on your system:

### Required Software

1. **Node.js** (v18 or higher)
2. **npm** (Node Package Manager)
3. **MongoDB** (Local installation or MongoDB Atlas account)
4. **Git** (for version control)

### Optional (for containerization)

5. **Docker** (for containerized deployment)
6. **Kubernetes** (kubectl and a cluster - for K8s deployment)

### Node.js Installation

#### Windows
1. Download the installer from [official Node.js website](https://nodejs.org/)
2. Run the installer and follow the installation wizard
3. Verify installation:
   ```bash
   node --version
   npm --version
   ```

#### Linux (Ubuntu/Debian)
```bash
sudo apt update
sudo apt install nodejs npm
node --version
npm --version
```

#### macOS
```bash
brew install node
node --version
npm --version
```

### MongoDB Setup

#### Option 1: MongoDB Atlas (Cloud - Recommended)
1. Create a free account at [MongoDB Atlas](https://www.mongodb.com/cloud/atlas)
2. Create a new cluster
3. Get your connection string (URI)
4. Create a database user with username and password

#### Option 2: Local MongoDB
```bash
# Ubuntu/Debian
sudo apt install mongodb

# macOS
brew install mongodb-community

# Start MongoDB service
sudo systemctl start mongodb  # Linux
brew services start mongodb-community  # macOS
```

---

## 🚀 Installation

### Step 1: Clone the Repository

```bash
git clone https://github.com/Rajeshgupta123456789/solar-system.git
cd solar-system
```

### Step 2: Install Dependencies

```bash
npm install
```

This will install all required packages listed in `package.json`:
- express (v4.18.2) - Web framework
- mongoose (v5.13.20) - MongoDB ODM
- cors (v2.8.5) - CORS middleware
- mocha, chai, chai-http - Testing libraries
- nyc - Code coverage tool

---

## 🏃 Running the Application

### Step 1: Set Environment Variables

Create a `.env` file in the root directory or export the following environment variables:

```bash
# MongoDB Configuration
export MONGO_URI="mongodb+srv://your-cluster.mongodb.net/superData"
export MONGO_USERNAME="your-username"
export MONGO_PASSWORD="your-password"
```

**For Windows PowerShell:**
```powershell
$env:MONGO_URI="mongodb+srv://your-cluster.mongodb.net/superData"
$env:MONGO_USERNAME="your-username"
$env:MONGO_PASSWORD="your-password"
```

**For Windows CMD:**
```cmd
set MONGO_URI=mongodb+srv://your-cluster.mongodb.net/superData
set MONGO_USERNAME=your-username
set MONGO_PASSWORD=your-password
```

### Step 2: Start the Application

```bash
npm start
```

The server will start on port **3000**.

### Step 3: Access the Application

Open your web browser and navigate to:
```
http://localhost:3000
```

You should see the Solar System application with a beautiful animated interface!

---

## 🧪 Testing

### Run Unit Tests

```bash
npm test
```

This will execute the test suite using Mocha and generate a test report (`test-results.xml`).

### Run Code Coverage

```bash
npm run coverage
```

This command will:
- Execute all unit tests
- Generate coverage reports in multiple formats:
  - **Cobertura** format (for CI/CD integration)
  - **LCOV** format (for detailed coverage analysis)
  - **Text** format (console output)
  - **JSON summary** format

Coverage reports will be available in the `coverage/` directory.

**Coverage Threshold:** The project is configured to require at least **90% code coverage**.

---

## 🐳 Docker Deployment

### Build Docker Image

```bash
docker build -t solar-system:latest .
```

### Run Docker Container

```bash
docker run -d \
  -p 3000:3000 \
  -e MONGO_URI="mongodb+srv://your-cluster.mongodb.net/superData" \
  -e MONGO_USERNAME="your-username" \
  -e MONGO_PASSWORD="your-password" \
  --name solar-system-app \
  solar-system:latest
```

### Verify Container is Running

```bash
docker ps
docker logs solar-system-app
```

### Access the Dockerized Application

Navigate to `http://localhost:3000` in your browser.

### Stop and Remove Container

```bash
docker stop solar-system-app
docker rm solar-system-app
```

---

## ☸️ Kubernetes Deployment

The project includes Kubernetes manifests for both **development** and **production** environments.

### Prerequisites

1. A running Kubernetes cluster (Minikube, EKS, GKE, AKS, etc.)
2. `kubectl` CLI installed and configured
3. Docker image pushed to a container registry (Docker Hub, ECR, etc.)

### Step 1: Create MongoDB Secret

```bash
kubectl create secret generic mongo-db-creds \
  --from-literal=MONGO_URI="mongodb+srv://your-cluster.mongodb.net/superData" \
  --from-literal=MONGO_USERNAME="your-username" \
  --from-literal=MONGO_PASSWORD="your-password" \
  -n <namespace>
```

### Step 2: Update Deployment Manifest

Edit `kubernetes/development/deployment.yaml` or `kubernetes/production/deployment.yaml`:

```yaml
# Replace placeholders with actual values
_{_NAMESPACE_}_    → your-namespace (e.g., development or production)
_{_REPLICAS_}_     → number of replicas (e.g., 2)
_{_IMAGE_}_        → your-docker-image (e.g., username/solar-system:latest)
```

### Step 3: Deploy to Kubernetes

#### Development Environment

```bash
kubectl apply -f kubernetes/development/deployment.yaml
kubectl apply -f kubernetes/development/service.yaml
kubectl apply -f kubernetes/development/ingress.yaml
```

#### Production Environment

```bash
kubectl apply -f kubernetes/production/deployment.yaml
kubectl apply -f kubernetes/production/service.yaml
kubectl apply -f kubernetes/production/ingress.yaml
```

### Step 4: Verify Deployment

```bash
# Check pods status
kubectl get pods -n <namespace>

# Check service
kubectl get svc -n <namespace>

# View logs
kubectl logs -f deployment/solar-system -n <namespace>
```

### Step 5: Access Application

```bash
# If using port-forward
kubectl port-forward svc/solar-system 3000:3000 -n <namespace>

# Then access: http://localhost:3000
```

### Health Checks

The application includes:
- **Readiness Probe**: `GET /` - Checks if app is ready to serve traffic
- **Liveness Probe**: `GET /` - Checks if app is alive and healthy

---

## 🔄 CI/CD Pipeline

This project uses **GitHub Actions** for automated CI/CD workflows.

### Workflow Overview

The pipeline (`/.github/workflows/solar-system.yaml`) includes:

#### 1️⃣ Unit Testing
- Runs tests on multiple Node.js versions (18, 20)
- Uses MongoDB service container for integration tests
- Caches npm dependencies for faster builds
- Archives test results as artifacts

#### 2️⃣ Code Coverage
- Generates comprehensive coverage reports
- Runs in containerized environment
- Uploads coverage reports as artifacts (retained for 5 days)

#### 3️⃣ Docker Build & Push
- Builds Docker image with commit SHA as tag
- Tests the Docker image before pushing
- Pushes to Docker Hub registry
- Only runs if tests pass

#### 4️⃣ Deployment
- **Dev Deploy**: Triggers on `feature/*` branches → deploys to development
- **Prod Deploy**: Triggers on `main` branch → deploys to production
- Uses reusable workflow for deployment automation

### Triggering the Pipeline

The workflow automatically triggers on:
```yaml
push:
  branches:
    - 'feature/*'
    - 'main'
```

### Required GitHub Secrets

Configure these in your repository settings:

```
MONGO_PASSWORD          → MongoDB password
DOCKER_HUB_ACCESS_TOKEN → Docker Hub access token
ngrok_authtoken         → Ngrok authentication token (for tunneling)
mongodb_password        → MongoDB password for K8s deployment
```

### Required GitHub Variables

```
MONGO_USERNAME          → MongoDB username
DOCKER_HUB_USERNAME     → Docker Hub username
```

---

## 📁 Project Structure

```
solar-system/
│
├── .github/
│   └── workflows/
│       ├── solar-system.yaml        # Main CI/CD workflow
│       └── reuasable-workflow.yaml  # Reusable deployment workflow
│
├── kubernetes/
│   ├── development/
│   │   ├── deployment.yaml          # Dev deployment manifest
│   │   ├── service.yaml             # Dev service manifest
│   │   └── ingress.yaml             # Dev ingress manifest
│   └── production/
│       ├── deployment.yaml          # Prod deployment manifest
│       ├── service.yaml             # Prod service manifest
│       └── ingress.yaml             # Prod ingress manifest
│
├── images/                          # Static images for UI
│
├── app.js                           # Main application file
├── app-controller.js                # Application controller logic
├── app-test.js                      # Unit tests
├── index.html                       # Frontend HTML
├── Dockerfile                       # Docker build instructions
├── package.json                     # Node.js dependencies and scripts
├── package-lock.json                # Locked dependency versions
├── .dockerignore                    # Docker ignore rules
├── .gitignore                       # Git ignore rules
└── README.md                        # Project documentation
```

---

## 🔐 Environment Variables

| Variable | Description | Required | Example |
|----------|-------------|----------|---------|
| `MONGO_URI` | MongoDB connection URI | Yes | `mongodb+srv://cluster.mongodb.net/superData` |
| `MONGO_USERNAME` | MongoDB username | Yes | `admin` |
| `MONGO_PASSWORD` | MongoDB password | Yes | `securePassword123` |
| `NODE_ENV` | Environment (development/production) | No | `production` |

---

## 🌐 API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/` | Serves the main HTML page |
| `POST` | `/planet` | Retrieves planet data by ID (0-9) |
| `GET` | `/os` | Returns OS hostname and environment |
| `GET` | `/live` | Liveness probe endpoint |
| `GET` | `/ready` | Readiness probe endpoint |

### Example: Get Planet Data

```bash
curl -X POST http://localhost:3000/planet \
  -H "Content-Type: application/json" \
  -d '{"id": 3}'
```

---

## 🤝 Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

---

## 📄 License

This project is licensed under the **MIT License**.

**Author:** Siddharth Barahalikar  
**Contact:** barahalikar.siddharth@gmail.com  
**LinkedIn:** [linkedin.com/in/barahalikar-siddharth](https://www.linkedin.com/in/barahalikar-siddharth/)

---

## 🙏 Acknowledgments

- Solar System images and design inspiration
- MongoDB for database services
- Node.js and Express communities
- GitHub Actions for CI/CD automation

---

**⭐ If you find this project helpful, please consider giving it a star!**

