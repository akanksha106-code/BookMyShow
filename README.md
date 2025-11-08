

# 🚀 **Building a Production-Ready Movie Ticketing Platform with DevOps: A Complete Guide**

## **From Code to Cloud: Deploying a BookMyShow Clone with Jenkins, Docker, Kubernetes, and AWS EKS**

## 📋 **Table of Contents**

  * [Introduction]
  * [Project Overview]
  * [Architecture]
  * [Prerequisites]
  * [Part 1: Infrastructure Setup]
  * [Part 2: CI/CD Pipeline Setup]
  * [Part 3: Kubernetes Deployment]
  * [Part 4: Monitoring & Observability]
  * [Challenges & Solutions]
  * [Results & Achievements]
  * [Cost Analysis]
  * [Key Learnings]
  * [Conclusion]
    
## 🎯 **Introduction**

In this comprehensive guide, I'll walk you through building and deploying a production-grade movie ticketing platform using modern DevOps practices. This isn't just another "hello world" deployment – we're implementing a complete CI/CD pipeline with automated testing, security scanning, container orchestration, and real-time monitoring.

**What makes this project special?**

  * ✅ **Complete automation** from code commit to production
  * ✅ **Enterprise-grade security** with Trivy and SonarQube
  * ✅ **Scalable infrastructure** on AWS EKS
  * ✅ **Real-time monitoring** with Prometheus and Grafana
  * ✅ **Production-ready practices** and configurations

## 🎬 **Project Overview**

A **BookMyShow clone** - a movie ticket booking platform deployed using:

  * **Application:** Node.js-based web application
  * **CI/CD:** Jenkins pipeline with automated builds
  * **Containers:** Docker for application packaging
  * **Orchestration:** Kubernetes on AWS EKS
  * **Monitoring:** Prometheus + Grafana stack
  * **Security:** Trivy for vulnerability scanning, SonarQube for code quality

### Business Requirements

  * ⚡ **Fast deployments** - Under 15 minutes from commit to production
  * 🔒 **Security-first** - Automated security scanning at every stage
  * 📊 **Observable** - Real-time metrics and alerting
  * 🔄 **Highly available** - Multi-node deployment with auto-scaling
  * 💰 **Cost-effective** - Optimized resource utilization

### Tech Stack

| Category | Technology | Purpose |
| :--- | :--- | :--- |
| **Application** | Node.js | Runtime environment |
| **Version Control** | GitHub | Source code management |
| **CI/CD** | Jenkins | Build automation |
| **Code Quality** | SonarQube | Static code analysis |
| **Security** | Trivy | Vulnerability scanning |
| **Containerization**| Docker | Application packaging |
| **Registry** | Docker Hub | Container image storage |
| **Orchestration** | Kubernetes (EKS) | Container orchestration |
| **Cloud Provider** | AWS | Infrastructure hosting |
| **Monitoring** | Prometheus | Metrics collection |
| **Visualization** | Grafana | Metrics dashboards |
| **Notifications** | Gmail SMTP | Build notifications |

-----

## 🏗️ **Architecture**

### High-Level Architecture

┌─────────────────────────────────────────────────────────────┐
│                   DEVELOPER WORKFLOW                         │
└─────────────────────────────────────────────────────────────┘
                            ↓
                    [Git Push to Main]
                            ↓
┌─────────────────────────────────────────────────────────────┐
│                    JENKINS CI/CD PIPELINE                    │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐   │
│  │  Checkout│→ │SonarQube │→ │  Build   │→ │  Trivy   │   │
│  │   Code   │  │ Analysis │  │  & Test  │  │   Scan   │   │
│  └──────────┘  └──────────┘  └──────────┘  └──────────┘   │
│       ↓              ↓              ↓              ↓         │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐   │
│  │  Docker  │→ │   Push   │→ │  Deploy  │→ │  Email   │   │
│  │  Build   │  │ DockerHub│  │   to K8s │  │  Notify  │   │
│  └──────────┘  └──────────┘  └──────────┘  └──────────┘   │
└─────────────────────────────────────────────────────────────┘
                            ↓
┌─────────────────────────────────────────────────────────────┐
│                  AWS EKS CLUSTER (us-east-1)                │
│                                                               │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐     │
│  │ Worker Node 1│  │ Worker Node 2│  │ Worker Node 3│     │
│  │  t3.medium   │  │  t3.medium   │  │  t3.medium   │     │
│  │              │  │              │  │              │     │
│  │ ┌──────────┐ │  │ ┌──────────┐ │  │ ┌──────────┐ │     │
│  │ │   Pod    │ │  │ │   Pod    │ │  │ │   Pod    │ │     │
│  │ │  BMS App │ │  │ │  BMS App │ │  │ │  BMS App │ │     │
│  │ └──────────┘ │  │ └──────────┘ │  │ └──────────┘ │     │
│  └──────────────┘  └──────────────┘  └──────────────┘     │
│                            ↓                                 │
│                  ┌──────────────────┐                       │
│                  │  Load Balancer   │                       │
│                  │    (AWS ELB)     │                       │
│                  └──────────────────┘                       │
└─────────────────────────────────────────────────────────────┘
                            ↓
                    [End Users Access]
                            ↓
                  http://your-app-url.com

┌─────────────────────────────────────────────────────────────┐
│              MONITORING INFRASTRUCTURE                        │
│                                                               │
│  ┌──────────────────────────────────────────────────────┐   │
│  │                   Prometheus Server                   │   │
│  │              (Metrics Collection & Storage)           │   │
│  └──────────────────────────────────────────────────────┘   │
│         ↑                ↑                ↑                   │
│         │                │                │                   │
│  ┌──────────┐    ┌──────────┐    ┌──────────┐              │
│  │  Node    │    │ Jenkins  │    │   EKS    │              │
│  │ Exporter │    │ Metrics  │    │ Metrics  │              │
│  │ :9100    │    │ :8080    │    │          │              │
│  └──────────┘    └──────────┘    └──────────┘              │
│         ↓                                                     │
│  ┌──────────────────────────────────────────────────────┐   │
│  │              Grafana Dashboards                       │   │
│  │         (Visualization & Alerting)                    │   │
│  └──────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────┘

### Infrastructure Components

1.  **Development Server (BMS-Server)**

      * **Purpose:** Hosts Jenkins, Docker, and development tools
      * **Instance:** EC2 `t2.large`
      * **OS:** Ubuntu 24.04 LTS
      * **Applications:** Jenkins, Docker, Trivy, AWS CLI, kubectl, eksctl

2.  **EKS Cluster (bms-eks)**

      * **Control Plane:** Managed by AWS
      * **Worker Nodes:** 3x `t3.medium` instances
      * **Networking:** VPC with public/private subnets across 2 AZs (`us-east-1a`, `us-east-1b`)

3.  **Monitoring Server**

      * **Purpose:** Hosts Prometheus and Grafana
      * **Instance:** EC2 `t2.medium`
      * **OS:** Ubuntu 22.04 LTS
      * **Components:** Prometheus, Node Exporter, Grafana


-----

## 🚀 **Part 1: Infrastructure Setup**

### Step 1.1: Launch Development Server

Create EC2 Instance:

  * **Name:** `BMS-Server` (The PDF uses `Akanksha`)
  * **AMI:** Ubuntu 24.04 LTS
  * **Instance Type:** `t2.large`
  * **Storage:** 28 GB
  * **Security Group (Inbound):**
      * Port 22 (SSH) - Your IP
      * Port 80 (HTTP) - 0.0.0.0/0
      * Port 443 (HTTPS) - 0.0.0.0/0
      * Port 8080 (Jenkins) - 0.0.0.0/0
      * Port 9000 (SonarQube) - 0.0.0.0/0
      * Port 3000-10000 (Custom) - 0.0.0.0/0

### Step 1.2: Create IAM User for EKS

  * **User name:** `eks-admin` 
  * **Attach policies:**
      * `AmazonEC2FullAccess`
      * `AmazonEKS_CNI_Policy` 
      * `AmazonEKSClusterPolicy`
      * `AmazonEKSWorkerNodePolicy`
      * `AWSCloudFormationFullAccess`
      * `IAMFullAccess` 
  * **Create Access Keys** and save them.

### Step 1.3: Install Essential Tools

Connect to your `BMS-Server` via SSH and install the following:

**1. Install AWS CLI:**

```bash
# Update system
sudo apt update

# Download and install AWS CLI
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
sudo apt install unzip -y
unzip awscliv2.zip
sudo ./aws/install

# Configure AWS credentials from Step 1.2
aws configure
AWS Access Key ID [None]: xxxxxxxxxx
AWS Secret Access Key [None]: xxxxxxxx
Default region name [None]: us-east-1
Default output format [None]: json

# Verify
aws sts get-caller-identity
```

**2. Install kubectl (v1.30 to match EKS cluster)**

```bash
# Download the kubectl binary for version 1.30
curl -O "https://s3.us-west-2.amazonaws.com/amazon-eks/1.30.0/2024-05-12/bin/linux/amd64/kubectl"

# Make it executable
chmod +x ./kubectl

# Move it to your path
sudo mv ./kubectl /usr/local/bin

# Verify installation
kubectl version --client
# Client Version: v1.30.0
```

**3. Install eksctl:**

```bash
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin
eksctl version
```

### Step 1.4: Create EKS Cluster

**Phase 1: Create Control Plane**

```bash
# This command creates the EKS control plane without any worker nodes
eksctl create cluster \
  --name=bms-eks \
  --region=us-east-1 \
  --zones=us-east-1a,us-east-1b \
  --version=1.30 \
  --without-nodegroup
```

**Phase 2: Associate IAM OIDC Provider**

```bash
eksctl utils associate-iam-oidc-provider \
    --region us-east-1 \
    --cluster bms-eks \
    --approve
```

**Phase 3: Create Node Group**

```bash
# Replace 'YourKeyPair' with your actual key name (without .pem)
eksctl create nodegroup \
  --cluster=bms-eks \
  --region=us-east-1 \
  --name=worker-nodes \
  --node-type=t3.medium \
  --nodes=3 \
  --nodes-min=2 \
  --nodes-max=4 \
  --ssh-access \
  --ssh-public-key=YourKeyPair \
  --managed \
  --full-ecr-access \
  --alb-ingress-access
```

**Phase 4: Verify Cluster**

```bash
kubectl get nodes
# This should show 3 nodes in 'Ready' state
```

-----

## 🛠️ **Part 2: CI/CD Pipeline Setup**

### Step 2.1: Install Jenkins

Create an installation script `jenkins.sh`:

```bash
#!/bin/bash

# Install Java 17 (verified from PDF)
sudo apt install openjdk-17-jre-headless -y

# Add Jenkins repository
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null

# Install Jenkins
sudo apt-get update
sudo apt-get install jenkins -y
```

Execute the script:

```bash
chmod +x jenkins.sh
./jenkins.sh

# Get initial password
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

**Access Jenkins:** `http://YOUR-SERVER-IP:8080` 

### Step 2.2: Install Docker

Create a `docker.sh` script:

```bash
#!/bin/bash
sudo apt-get update
sudo apt-get install -y ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
sudo apt-get install -y docker-ce docker-ce-cli containerd.io
```

Execute and configure:

```bash
chmod +x docker.sh
./docker.sh

# Add jenkins user to the docker group (Solution from Challenge 3)
sudo usermod -aG docker jenkins

# Fix permissions (using 656 as seen in PDF/Challenge 3)
sudo chmod 656 /var/run/docker.sock

# Restart Jenkins to pick up the new group membership
sudo systemctl restart jenkins

# Test Docker
docker --version
# Docker version 28.5.2

# Login to Docker Hub
docker login -u akankshatech
# Login Succeeded
```

### Step 2.3: Install Trivy (Security Scanner)

Create `trivy.sh`:

```bash
#!/bin/bash
sudo apt-get install wget apt-transport-https gnupg -y
wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | gpg --dearmor | sudo tee /usr/share/keyrings/trivy.gpg > /dev/null
echo "deb [signed-by=/usr/share/keyrings/trivy.gpg] https://aquasecurity.github.io/trivy-repo/deb generic main" | sudo tee -a /etc/apt/sources.list.d/trivy.list
sudo apt-get update
sudo apt-get install trivy -y
```

Execute:

```bash
chmod +x trivy.sh
./trivy.sh
trivy --version
```

### Step 2.4: Setup SonarQube

```bash
# Run SonarQube container
docker run -d --name sonar -p 9000:9000 sonarqube:lts-community

# Verify
docker ps

# Access: http://YOUR-SERVER-IP:9000 
# Default: admin/admin
```

### Step 2.5: Install Additional Tools

```bash
# Install Node.js/NPM (for 'Install Dependencies' stage)
sudo apt install npm -y
npm --version
```

### Step 2.6: Configure Jenkins

  * **Install Required Plugins:**
      * `Eclipse Temurin Installer`
      * `SonarQube Scanner`
      * `NodeJS`
      * `Docker`, `Docker Pipeline`
      * `Kubernetes`, `Kubernetes CLI`
      * `OWASP Dependency-Check`
      * `Pipeline Stage View`
      * `Email Extension Template`
      * `Prometheus Metrics`
  * **Configure Tools (Manage Jenkins → Tools):**
      * **JDK:** `jdk17`
      * **NodeJS:** `node23`
      * **SonarQube Scanner:** `sonar-scanner`
  * **Configure SonarQube Integration (Manage Jenkins → System):**
      * Add SonarQube server (`sonar-server`)
      * URL: `http://localhost:9000`
  * **Configure Credentials (Manage Jenkins → Credentials):**
      * **SonarQube Token:** Go to SonarQube, generate a token. In Jenkins, add `Secret text` credentials. **ID:** `Sonar-token`. **Secret:** (Your-SonarQube-Token)
      * **Docker Hub:** Add `Username with password` credentials. **ID:** `docker`. **Username:** `akankshatech`. **Password:** (Your-DockerHub-Password/Token)
  * **Configure Email Notifications:**
      * Use a Gmail account with an **App Password**.
      * Configure `Extended E-mail Notification` and `E-mail Notification` with:
          * **SMTP server:** `smtp.gmail.com`
          * **SMTP Port:** `465`
          * **Credentials:** Your Gmail username and App Password
          * **Use SSL:** Yes

### Step 2.7: Create Jenkins Pipeline

**1. Configure Jenkins for Kubernetes Access:**
The Jenkins user needs access to `kubectl`.

```bash
# Switch to jenkins user
sudo -su jenkins

# Configure AWS credentials
aws configure
# (Enter the same IAM keys as before)

# Update kubeconfig for the jenkins user
aws eks update-kubeconfig --name bms-eks --region us-east-1

# Test kubectl
kubectl get nodes

# Exit jenkins user and restart Jenkins
exit
sudo systemctl restart jenkins
```

**2. Create Kubernetes Manifests:**
Add `deployment.yml` and `service.yml` to your Git repository.


**3. Complete Jenkins Pipeline:**
Create a new Pipeline job named `BMS-App` .


## 📊 **Part 4: Monitoring & Observability**

### Step 4.1: Launch Monitoring Server

  * **Name:** `Monitoring-Server`
  * **AMI:** Ubuntu 22.04 LTS
  * **Instance Type:** `t2.medium`
  * **Security Group:** Open ports `9090` (Prometheus), `9100` (Node Exporter), `3000` (Grafana)

### Step 4.2: Install Prometheus

Connect to the monitoring server and set up Prometheus.
*(For brevity, commands to create users and service files are simplified. Refer to the template for full scripts.)*

1.  Download and install Prometheus.
2.  Create `prometheus.service` file.
3.  Start the service: `sudo systemctl start prometheus`
4.  **Access:** `http://server:9090`

### Step 4.3: Install Node Exporter

1.  Download and install Node Exporter on the monitoring server.
2.  Create `node_exporter.service` file.
3.  Start the service: `sudo systemctl start node_exporter`

### Step 4.4: Configure Prometheus Scraping

Edit `/etc/prometheus/prometheus.yml` to add scrape targets:

```yaml
scrape_configs:
  - job_name: 'prometheus'
    static_configs:
      - targets: ['localhost:9090']

  - job_name: 'node_exporter'
    static_configs:
      # This server's IP
      - targets: ['monitoring-server:9100']

  - job_name: 'jenkins'
    metrics_path: '/prometheus'
    static_configs:
      # Your Jenkins/Dev Server IP
      - targets: ['jenkins-server:8080']
```

Reload Prometheus: `curl -X POST http://localhost:9090/-/reload`
**Verify Targets:** `http://monitoring:9090/targets`

### Step 4.5: Install Grafana

1.  Install Grafana using the `apt` repository.
2.  Start the service: `sudo systemctl start grafana-server`
3.  **Access:** `http://monitoring:3000` (Default: `admin`/`admin`)

### Step 4.6: Configure Grafana Dashboards

1.  **Add Data Source:**
      * Configuration → Data Sources → Add Prometheus
      * **URL:** `http://localhost:9090`
      * Save & Test
2.  **Import Dashboards:**
      * **Dashboard 1: Node Exporter Full**
          * ID: `1860`
          * Click Import → Load → Select Prometheus → Import
      * **Dashboard 2: Jenkins Performance** ("Jenkins Health Overview")
          * ID: `9964`
          * Click Import → Load → Select Prometheus → Import

-----

## 🎯 **Challenges & Solutions**

  * **Challenge 1: LoadBalancer Not Accessible**

      * **Problem:** After deploying `bms-service`, the `EXTERNAL-IP` was created but the URL timed out.
      * **Solution:** The Security Group associated with the AWS ELB did not allow inbound traffic on port 80. I had to manually edit the ELB's security group to allow `HTTP` traffic from `0.0.0.0/0`.

  * **Challenge 2: Jenkins Unable to Access EKS Cluster**

      * **Problem:** The pipeline failed at the `Deploy to EKS` stage with authentication errors.
      * **Solution:** The Jenkins service runs as the `jenkins` user, which did not have the AWS credentials or kubeconfig. The fix was to `sudo -su jenkins`, run `aws configure`, and then run `aws eks update-kubeconfig --name bms-eks ...` to create a kubeconfig file for the `jenkins` user.

  * **Challenge 3: Docker Permission Denied**

      * **Problem:** Jenkins pipeline failed with "permission denied... docker.sock".
      * **Solution:** The `jenkins` user was not in the `docker` group.
          * `sudo usermod -aG docker jenkins`
          * `sudo chmod 656 /var/run/docker.sock` (as seen in the PDF)
          * `sudo systemctl restart jenkins`

-----

## 📈 **Results & Achievements**

  * **Performance Metrics:**
      * **Deployment Time:** `~6.5 minutes` (from `6min 36s` seen in Jenkins)
  * **Key Achievements:**
      * ✅ **Fully Automated Pipeline:** Zero-touch deployment from `git push` to live on EKS.
      * ✅ **Scalable Infrastructure:** Application running on a 3-node auto-scaling Kubernetes cluster.
      * ✅ **Security Implementation:**
          * **SonarQube Quality Gate: `Passed`**
          * **Bugs:** `18`
          * **Vulnerabilities:** `0`
          * **Code Smells:** `116`
          * **Trivy FS Scan:** Integrated into the pipeline for filesystem vulnerability scanning.
      * ✅ **Complete Observability:** Real-time monitoring of both the Jenkins server and application nodes using Prometheus and Grafana.
      * ✅ **High Availability:** Deployed with a `LoadBalancer` service distributing traffic across 3 pods on 3 different nodes.

-----

## 💰 **Cost Analysis**

### Monthly AWS Cost Breakdown (Estimate)

| Resource | Specification | Quantity | Cost/Month (Est.) |
| :--- | :--- | :--- | :--- |
| **EKS Control Plane** | Managed | 1 | \~$73 |
| **Worker Nodes** | `t3.medium` | 3 | ~$95 |
| **BMS Server** | `t2.large` | 1 | \~$68 |
| **Monitoring Server**| `t2.medium` | 1 | ~$34 |
| **EBS Volumes** | gp3 | \~88 GB total | \~$7 |
| **LoadBalancer** | ELB | 1 | ~$25 |
| **Total** | | | **\~$302/month** |

### Cost Optimization Strategies

  * **Reserved Instances:** Can be used for the `t2.large` and `t2.medium` servers to save \~40%.
  * **Auto-scaling:** The EKS node group is configured to scale down to 2 nodes during off-hours.
  * **EBS Optimization:** Using `gp3` volumes provides better performance at a lower cost than `gp2`.

-----

## 🎓 **Key Learnings**

  * **Security Should Be Automated:** Integrating SonarQube and Trivy directly into the pipeline catches issues *before* they reach production. Seeing the "0 Vulnerabilities" result from SonarQube is a tangible win.
  * **Monitoring is Non-Negotiable:** Setting up Prometheus and Grafana provided instant insight into Jenkins performance (like memory usage) and node health.
  * **IAM & User Permissions are Tricky:** The "permission denied" errors (Docker socket, `kubectl`) highlight that every service (like Jenkins) has its own user context that must be configured.
  * **Infrastructure as Code Matters:** Defining the deployment in `deployment.yml` and `service.yml` makes it repeatable, version-controlled, and easy to manage.

-----

## 🎬 **Conclusion**

This project successfully demonstrates a complete, production-ready DevOps workflow. By integrating CI/CD (Jenkins), containerization (Docker), orchestration (EKS), security (SonarQube, Trivy), and monitoring (Prometheus, Grafana), we built a system that is automated, scalable, secure, and observable.

The final result is a "BookMyShow" clone, deployed on a load-balanced Kubernetes cluster, with all metrics visible on a Grafana dashboard—all triggered by a single `git push`.
