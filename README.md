# StartTech App

A simple **full-stack Todo application** built as a **learning, assessment, and portfolio project**.

This project focuses on modern full-stack development practices and CI/CD automation. The application repository was **forked and extended** from the original project below:

> Base project: [https://github.com/Innocent9712/much-to-do/tree/feature/full-stack](https://github.com/Innocent9712/much-to-do/tree/feature/full-stack)

---

## 🚀 Project Overview

StartTech App is a basic todo application that allows users to manage tasks through a web interface backed by a RESTful API.

The goal of this project is to:

* Practice full-stack development
* Learn containerized application workflows
* Implement comprehensive CI/CD pipelines with GitHub Actions
* Build a portfolio-ready DevOps-oriented project

---

## 🛠 Tech Stack

### Backend

* **Golang**
* REST API

### Frontend

* **React**
* **TypeScript**

### CI/CD

## ⚙️ CI/CD with GitHub Actions

This project uses **GitHub Actions** to implement **automated continuous integration and deployment** for both the backend and frontend components. The workflows ensure that every push to the `main` branch triggers tests, security checks, Docker image builds, and deployments to AWS.

### 1️⃣ Backend CI/CD Workflow

**Workflow file:** `.github/workflows/backend-ci-cd.yml`

**Triggers:**

* Push to `main` branch affecting `backend/**`

**Environment Variables & Secrets:**

* `AWS_REGION`, `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY` – for AWS deployment
* `DOCKER_USERNAME`, `DOCKER_PASSWORD` – to push images to Docker Hub
* `MONGO_URI`, `DB_NAME` – database connection variables
* `PROJECT_NAME` – used for naming Auto Scaling Groups and log groups

**Workflow Jobs:**

1. **Test**

   * Sets up Go environment (`1.22`)
   * Runs **unit and integration tests** (`go test ./...`)
   * Performs **code quality checks** with `go vet`
   * Scans Go dependencies with **Trivy** for HIGH/CRITICAL vulnerabilities

2. **Build**

   * Logs in to Docker Hub
   * Builds Docker image for the backend and pushes both `latest` and SHA-tagged versions
   * Scans the pushed image for vulnerabilities using Trivy

3. **Deploy**

   * Connects to AWS using the provided credentials
   * Queries all **running EC2 instances** in the Auto Scaling Group
   * Sends deployment commands via **AWS SSM RunCommand**

     * Pulls the latest Docker image
     * Stops and removes existing containers
     * Ensures a Docker network exists
     * Starts the backend container with logging to CloudWatch
     * Updates Nginx reverse proxy configuration
   * Automates deployment without direct SSH access

**Key Benefits:**

* Full automation of build → test → deploy pipeline
* Security scanning for dependencies and Docker images
* Dynamic deployment to multiple EC2 instances in an Auto Scaling Group
* Centralized logging with CloudWatch

---

### 2️⃣ Frontend CI/CD Workflow

**Workflow file:** `.github/workflows/frontend-ci-cd.yml`

**Triggers:**

* Push to `main` branch affecting `frontend/**`

**Environment Variables & Secrets:**

* `AWS_REGION`, `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY` – AWS access
* `S3_BUCKET_NAME` – static frontend hosting bucket
* `CLOUDFRONT_DIST_ID` – CloudFront distribution for global content delivery

**Workflow Jobs:**

1. **Build and Deploy**

   * Sets up Node.js (`v22.12`) environment
   * Installs frontend dependencies
   * Runs `npm audit fix` and `npm audit` to fix vulnerabilities and check security
   * Builds the React frontend (`npm run build`)
   * Syncs build artifacts to **S3 bucket**
   * Invalidates CloudFront cache to serve latest assets
   * Provides a deployment notification in workflow logs

**Key Benefits:**

* Fully automated frontend deployment to S3 + CloudFront
* Security checks integrated into the CI/CD pipeline
* Immediate cache invalidation ensures users see the latest version
* Clear separation between frontend and backend workflows

---

### 3️⃣ Architecture and DevOps Practices Demonstrated

* **Containerization:** Backend runs in Docker containers orchestrated on EC2
* **Infrastructure as Code:** AWS resources managed with Terraform (in a separate repo)
* **Auto Scaling:** Backend deployment dynamically targets all instances in the Auto Scaling Group
* **SSM over SSH:** Secure instance management without exposing SSH
* **Continuous Security:** Dependency scanning and Docker image vulnerability checks
* **Separation of Concerns:** Frontend and backend pipelines operate independently

---

This workflow setup demonstrates **real-world DevOps practices** for a portfolio-ready full-stack project, integrating **testing, security, containerization, and automated cloud deployment** in a cohesive CI/CD strategy.


> ⚠️ Infrastructure (AWS, Terraform, Auto Scaling, monitoring) is managed in a **separate repository** and is intentionally excluded from this README.
Infrastructure Repository: [https://github.com/farmerpisco/starttech-infra-paul-1570](https://github.com/farmerpisco/starttech-infra-paul-1570)


---

## 🎯 Purpose of This Project

This repository is intended for:

* **Learning** (full-stack + DevOps concepts)
* **Assessments / technical evaluations**
* **Portfolio demonstration**

The focus is on clarity, correctness, and real-world DevOps practices rather than feature completeness.

---


---

## 👤 Author

**Paul Adegoke**
📧 Email: [farmerpisco@gmail.com](mailto:farmerpisco@gmail.com)

---
