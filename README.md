# 🚀 Automated CI/CD Pipeline for Web Application

![GitHub](https://img.shields.io/badge/GitHub-Repository-black?logo=github)
![GitHub Actions](https://github.com/Pravesh880082/devops-cicd-project/blob/521ca2d2ec6c5377cd319207fa6413661c2a64c7/Screenshot%202026-09-06%20100329.png)
![Docker](https://img.shields.io/badge/Docker-Containerized-2496ED?logo=docker)
![AWS](https://img.shields.io/badge/AWS-EC2-orange?logo=amazonaws)
![Nginx](https://img.shields.io/badge/Nginx-Web%20Server-green?logo=nginx)
![Linux](https://img.shields.io/badge/Linux-Ubuntu-black?logo=linux)

> A containerized static web application deployed on AWS EC2 using an automated CI/CD pipeline with GitHub Actions, Docker, SSH, and Nginx.

---

## 📌 Project Overview

This project demonstrates the implementation of a basic **DevOps CI/CD pipeline** for a static web application.

Whenever new code is pushed to the `main` branch, **GitHub Actions automatically builds the Docker image and deploys the latest version of the application to an AWS EC2 instance using SSH**.

The application runs inside a Docker container using **Nginx** as the web server.

### 🔄 Deployment Flow

```text
Developer
    │
    │ git push
    ▼
GitHub Repository
    │
    ▼
GitHub Actions
    │
    ├── Checkout Code
    │
    ├── Build Docker Image
    │
    └── SSH Deployment
             │
             ▼
         AWS EC2
             │
             ▼
      Docker Container
             │
             ▼
           Nginx
             │
             ▼
       Live Website
```

---

## 🎯 Project Objectives

The main objectives of this project are:

* Understand the fundamentals of CI/CD.
* Containerize a web application using Docker.
* Create an automated workflow using GitHub Actions.
* Deploy a Dockerized application on AWS EC2.
* Configure Nginx as a web server.
* Implement SSH-based automated deployment.
* Gain practical experience with Linux and cloud infrastructure.
* Understand how code moves from development to production.

---

## 🛠️ Technologies Used

| Technology     | Purpose                      |
| -------------- | ---------------------------- |
| HTML5          | Website structure            |
| CSS3           | Website styling              |
| Git            | Version control              |
| GitHub         | Source code management       |
| GitHub Actions | CI/CD automation             |
| Docker         | Application containerization |
| Nginx          | Web server                   |
| AWS EC2        | Cloud hosting                |
| Ubuntu Linux   | Server operating system      |
| SSH            | Secure server deployment     |

---

## ✨ Key Features

* ✅ Static responsive web application
* ✅ Dockerized application
* ✅ Nginx-based web server
* ✅ Automated Docker image build
* ✅ GitHub Actions CI/CD pipeline
* ✅ Automatic deployment to AWS EC2
* ✅ SSH-based remote deployment
* ✅ Cloud-hosted application
* ✅ Version-controlled source code
* ✅ Automatic container replacement during deployment

---

# 📂 Project Structure

```text
devops-cicd-project/
│
├── .github/
│   └── workflows/
│       ├── docker.yml
│       └── deploy.yml
│
├── screenshots/
│   ├── website.png
│   ├── github-actions.png
│   ├── docker.png
│   ├── ec2.png
│   └── live-site.png
│
├── index.html
├── style.css
├── Dockerfile
├── .gitignore
└── README.md
```

---

# 🔧 How the CI/CD Pipeline Works

## 1️⃣ Developer Pushes Code

The development process starts when changes are made to the website.

```bash
git add .
git commit -m "Update website"
git push origin main
```

The push to the `main` branch triggers GitHub Actions.

---

## 2️⃣ GitHub Repository

GitHub stores the complete source code and project configuration.

The repository contains:

* Website files
* Dockerfile
* GitHub Actions workflows
* README documentation

---

## 3️⃣ GitHub Actions

GitHub Actions automatically starts the CI/CD workflow after a push to `main`.

The CI pipeline performs the following task:

```text
Checkout Code
      ↓
Build Docker Image
      ↓
Verify Docker Build
```

Workflow file:

```text
.github/workflows/docker.yml
```

### CI Workflow

```yaml
name: Docker CI Pipeline

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  build:
    name: Build Docker Image
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Repository
        uses: actions/checkout@v4

      - name: Build Docker Image
        run: docker build -t devops-cicd-project:latest .
```

---

# 🐳 Docker Implementation

Docker is used to package the website and Nginx web server into a container.

### Dockerfile

```dockerfile
FROM nginx:alpine

COPY index.html /usr/share/nginx/html/index.html
COPY style.css /usr/share/nginx/html/style.css

EXPOSE 80
```

### Explanation

| Instruction         | Purpose                                  |
| ------------------- | ---------------------------------------- |
| `FROM nginx:alpine` | Uses lightweight Nginx image             |
| `COPY index.html`   | Copies website HTML into Nginx directory |
| `COPY style.css`    | Copies website CSS                       |
| `EXPOSE 80`         | Documents HTTP port 80                   |

---

# 🧪 Running Docker Locally

Build the Docker image:

```bash
docker build -t devops-cicd-project .
```

Run the container:

```bash
docker run -d -p 8080:80 --name devops-web devops-cicd-project
```

Check running containers:

```bash
docker ps
```

The application can then be accessed locally at:

```text
http://localhost:8080
```

---

# ☁️ AWS EC2 Deployment

The application is hosted on an **AWS EC2 Ubuntu instance**.

The EC2 server is responsible for:

* Running Docker
* Hosting the application
* Running the Nginx container
* Receiving automated deployments

### EC2 Configuration

```text
Operating System: Ubuntu
Web Server: Nginx
Container Runtime: Docker
Deployment Method: SSH
Application Port: 80
```

---

# 🔐 SSH Deployment

GitHub Actions connects to the EC2 instance using SSH.

The deployment workflow performs the following operations:

```text
GitHub Actions
      ↓
SSH Connection
      ↓
AWS EC2
      ↓
Pull Latest Code
      ↓
Build Docker Image
      ↓
Remove Previous Container
      ↓
Start New Container
```

---

# ⚙️ Automated Deployment Workflow

Deployment workflow file:

```text
.github/workflows/deploy.yml
```

```yaml
name: Deploy to AWS EC2

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Deploy to EC2
        uses: appleboy/ssh-action@v1.2.2
        with:
          host: ${{ secrets.EC2_HOST }}
          username: ${{ secrets.EC2_USERNAME }}
          key: ${{ secrets.EC2_SSH_KEY }}

          script: |
            cd ~/devops-cicd-project
            git pull origin main
            sudo docker build -t devops-cicd-project .
            sudo docker rm -f devops-web || true
            sudo docker run -d -p 80:80 --name devops-web devops-cicd-project
```

---

# 🔑 GitHub Secrets

Sensitive deployment information is stored securely using **GitHub Repository Secrets**.

The following secrets are configured:

```text
EC2_HOST
EC2_USERNAME
EC2_SSH_KEY
```

### Purpose

| Secret         | Purpose                             |
| -------------- | ----------------------------------- |
| `EC2_HOST`     | EC2 public IP address               |
| `EC2_USERNAME` | EC2 SSH username                    |
| `EC2_SSH_KEY`  | Private SSH key used for deployment |

> ⚠️ **Security:** Private SSH keys, passwords, API keys, and other credentials must never be committed to the repository.

---

# 🔒 Security Configuration

The EC2 Security Group allows the required network traffic.

Recommended configuration:

| Type | Port | Source      |
| ---- | ---: | ----------- |
| SSH  |   22 | My IP       |
| HTTP |   80 | `0.0.0.0/0` |

SSH access should ideally be restricted to your own IP address instead of exposing port `22` to the entire internet.

---

# 🌐 Live Application

The application is deployed on AWS EC2.

### Live Demo

```text
http://YOUR_EC2_PUBLIC_IP
```

> Replace `YOUR_EC2_PUBLIC_IP` with the current public IP/domain of your EC2 instance.

---

# 📸 Screenshots

## 🖥️ Website

![Website](screenshots/website.png)

---

## ⚙️ GitHub Actions

![GitHub Actions](screenshots/github-actions.png)

---

## 🐳 Docker Container

![Docker](screenshots/docker.png)

---

## ☁️ AWS EC2

![AWS EC2](screenshots/ec2.png)

---

## 🌐 Live Website

![Live Website](screenshots/live-site.png)

---

# 🔄 Complete CI/CD Process

The complete deployment process can be summarized as:

```text
1. Developer modifies website
             ↓
2. git add / commit / push
             ↓
3. GitHub receives changes
             ↓
4. GitHub Actions is triggered
             ↓
5. Docker image is built
             ↓
6. GitHub Actions connects to EC2
             ↓
7. Latest code is pulled
             ↓
8. Docker image is rebuilt
             ↓
9. Previous container is removed
             ↓
10. New container is started
             ↓
11. Nginx serves the website
             ↓
12. Updated website becomes live
```

---

# 🧪 Testing the Pipeline

To verify automatic deployment:

### Step 1 — Modify Website

Edit:

```text
index.html
```

For example, change the hero heading.

### Step 2 — Commit Changes

```bash
git add .
git commit -m "Update website content"
```

### Step 3 — Push to GitHub

```bash
git push origin main
```

### Step 4 — Check GitHub Actions

Open the repository's **Actions** tab.

The deployment workflow should execute successfully.

```text
Build Docker Image       ✓
SSH Deployment           ✓
Docker Container         ✓
Deployment               ✓
```

### Step 5 — Refresh Website

Open:

```text
http://YOUR_EC2_PUBLIC_IP
```

The updated website should appear automatically.

---

# 📊 Project Architecture

```text
                  ┌─────────────────┐
                  │    Developer    │
                  └────────┬────────┘
                           │
                      git push
                           │
                           ▼
                  ┌─────────────────┐
                  │     GitHub      │
                  │   Repository    │
                  └────────┬────────┘
                           │
                           ▼
                  ┌─────────────────┐
                  │ GitHub Actions  │
                  │                 │
                  │ Docker Build    │
                  │ SSH Deployment  │
                  └────────┬────────┘
                           │
                       SSH / Code
                           │
                           ▼
                  ┌─────────────────┐
                  │    AWS EC2      │
                  │     Ubuntu      │
                  └────────┬────────┘
                           │
                           ▼
                  ┌─────────────────┐
                  │ Docker Container│
                  │                 │
                  │     Nginx      │
                  └────────┬────────┘
                           │
                         HTTP
                           │
                           ▼
                  ┌─────────────────┐
                  │  Live Website   │
                  └─────────────────┘
```

---

# 📚 What I Learned

Through this project, I gained practical experience in:

* Git and GitHub version control
* GitHub Actions
* CI/CD concepts
* Docker containerization
* Docker images and containers
* Linux server administration
* SSH authentication
* AWS EC2 deployment
* Nginx configuration
* Security Group configuration
* Automated deployment
* Cloud-based application hosting

---

# 🚀 Future Improvements

The project can be further improved by implementing:

* 🔹 Docker Hub / Amazon ECR integration
* 🔹 HTTPS using SSL/TLS
* 🔹 Custom domain name
* 🔹 AWS Application Load Balancer
* 🔹 AWS Route 53
* 🔹 Infrastructure as Code using Terraform
* 🔹 Monitoring using AWS CloudWatch
* 🔹 Docker image versioning
* 🔹 Rollback mechanism
* 🔹 Automated testing
* 🔹 Multi-stage Docker builds
* 🔹 Zero-downtime deployment
* 🔹 Production-grade Nginx configuration

---

# 💡 Key DevOps Concepts Demonstrated

This project demonstrates the following DevOps practices:

```text
Version Control
      ↓
Continuous Integration
      ↓
Containerization
      ↓
Continuous Deployment
      ↓
Cloud Infrastructure
      ↓
Automation
```

---

# 📁 Important Files

| File         | Description                       |
| ------------ | --------------------------------- |
| `index.html` | Main website                      |
| `style.css`  | Website styling                   |
| `Dockerfile` | Docker image configuration        |
| `docker.yml` | Docker CI workflow                |
| `deploy.yml` | EC2 deployment workflow           |
| `.gitignore` | Prevents sensitive/unwanted files |
| `README.md`  | Project documentation             |

---

# 👨‍💻 Author

**Pravesh Kumar**

Aspiring Cloud & DevOps Engineer

### Connect With Me

* GitHub: `https://github.com/pravesh880082`
* LinkedIn: `https://www.linkedin.com/in/pravesh-kumar-63b625372`

---

# ⭐ Support

If you found this project useful or interesting, consider giving the repository a ⭐ on GitHub.

---

## 📄 License

This project is created for **learning, practice, and educational purposes**.

You are free to explore and modify the project for your own learning.
