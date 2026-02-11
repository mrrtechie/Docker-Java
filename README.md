# Docker-Java
---

#  Enterprise DevSecOps CI/CD Pipeline for Java Application

## Project Overview

This project demonstrates the implementation of a complete **end-to-end DevSecOps CI/CD pipeline** for a Java-based web application using Jenkins.

The pipeline automates:

* Source code checkout from GitHub
* Build and packaging using Maven
* Static Code Analysis using SonarQube
* Quality Gate validation
* Artifact upload to Nexus Repository
* Docker image creation (Application + Database)
* Container vulnerability scanning using Trivy
* Deployment using Docker Compose
* Email notification after pipeline execution
* Application and container monitoring with New Relic

---

##  Tech Stack

* Git
* Jenkins (Declarative Pipeline)
* Maven
* SonarQube
* Nexus Repository (Nexus3)
* Docker (Tomcat container)
* Docker Compose
* Trivy (Security Scanning)
* OWASP (dependency analysis)
* Email Extension Plugin
* New Relic (Monitoring)
* AWS EC2 (Amazon Linux)

---

##  Infrastructure Setup

### EC2 Instances

* **CI Server Instance**

  * Jenkins
  * Docker
  * SonarQube (running in container)
  * Trivy

* **Nexus Repository Instance**

  * Dedicated EC2 instance
  * Nexus3 installed manually

---

##  Jenkins Pipeline Stages

Pipeline definition: 

### 1. Clean Workspace

Removes previous build artifacts using `cleanWs()`.

### 2. Code Checkout

Clones Java application from GitHub repository.

### 3. Build

* Executes:

  ```
  mvn clean package -DskipTests
  ```
* Copies WAR file into Docker build context.

### 4. Code Quality Analysis (CQA)

* Integrated SonarQube using `withSonarQubeEnv`
* Runs:

  ```
  mvn clean package sonar:sonar -DskipTests -Dsonar.projectKey=Java
  ```
* Sonar webhook configured for Jenkins Quality Gate validation.

### 5. Artifact Upload

Uploads generated WAR file to Nexus repository using:

* groupId: `com.visualpathit`
* artifactId: `vprofile`
* version: `v2`
* repository: `java`

### 6. Docker Image Build

Builds two images:

* `appimage` (Application container)
* `dbimage` (Database container)

### 7. Image Security Scan

Scans Docker images using Trivy:

```
trivy image appimage
trivy image dbimage
```

Reports are stored as:

* `appimage-report.txt`
* `dbimage-report.txt`

### 8. Deployment

Uses Docker Compose inside `compose` directory:

```
docker compose up -d
```

Deploys multi-container application.

---

##  Security Implementation

* SonarQube static code analysis
* Quality Gate enforcement
* Trivy container vulnerability scanning
* OWASP dependency checking
* Nexus repository artifact versioning
* Secure SMTP configuration with App Password

---

## Artifact Repository

Artifacts are uploaded to Nexus 3 repository.

* Hosted repository
* Redeploy allowed
* Centralized artifact management

---

## Monitoring

Integrated **New Relic** for:

* Host monitoring
* Container monitoring
* Application performance tracking

---

## Notifications

Pipeline configured with Email Extension Plugin.

After every build:

* Sends build result
* Includes job name
* Includes build number
* Includes build URL

---


---


