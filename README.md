# Enterprise DevSecOps CI/CD Pipeline with Jenkins, Kubernetes, SonarQube, Snyk, Docker, Trivy & AWS EKS

## Project Overview

This project demonstrates a **complete end-to-end DevSecOps CI/CD pipeline** designed to replicate the **real-world DevOps pipeline architecture used in enterprise environments**.

The pipeline automates the process of building, scanning, containerizing, and deploying a **Java-based Spring Boot application** into a **Kubernetes cluster running on AWS EKS**.

Security is integrated at multiple stages of the pipeline using modern DevSecOps tools, ensuring that vulnerabilities are detected early in the software delivery lifecycle.

This implementation helps simulate how organizations manage **secure CI/CD workflows across development environments such as Dev, QA, and Production**.

---

## Architecture Workflow

Developer Commit
↓
GitHub Repository
↓
Jenkins Pipeline Trigger
↓
Slack Notification – Pipeline Started
↓
Maven Build
↓
SonarQube Static Code Analysis
↓
Snyk Dependency Security Scan
↓
Docker Image Build
↓
Trivy Container Image Scan
↓
Push Image to DockerHub
↓
Deploy to AWS EKS Kubernetes Cluster
↓
Trivy Kubernetes Configuration Scan
↓
Application Deployment via NodePort
↓
Slack Notification – Pipeline Result

---

## Tools & Technologies Used

* Jenkins – CI/CD pipeline orchestration
* GitHub – Source code management
* Jenkinsfile – Pipeline as Code
* Maven – Java build automation tool
* SonarQube – Static code analysis
* Snyk – Dependency vulnerability scanning
* Docker – Containerization platform
* DockerHub – Container image registry
* Trivy – Container and Kubernetes security scanning
* AWS EKS – Managed Kubernetes service
* Kubernetes – Container orchestration platform
* AWS CLI – AWS service interaction
* kubectl – Kubernetes command line tool
* Jenkins Shared Library – Reusable pipeline logic
* Slack – Pipeline notification system
* Spring Boot – Backend application framework
* MongoDB – Database

---

## Pipeline as Code (Jenkinsfile)

The entire CI/CD pipeline is defined using a **Jenkinsfile**, following the **Pipeline as Code** approach.

The Jenkinsfile is stored inside the **GitHub repository along with the application source code**.

Benefits of using Jenkinsfile:

* Version-controlled pipeline configuration
* Reproducible CI/CD workflows
* Easier collaboration between developers and DevOps engineers
* Simplified pipeline maintenance

When Jenkins triggers the pipeline, it automatically reads the **Jenkinsfile from GitHub** and executes each stage sequentially.

---

## Continuous Integration (CI) Pipeline

### 1. Source Code Integration

The pipeline begins when a developer commits code changes to the GitHub repository.

Jenkins automatically pulls the latest source code.

---

### 2. Build Stage

The application is built using **Maven**, since the project is a **Java Spring Boot application**.

Build command used:

mvn clean package

Maven is configured directly inside the Jenkins server.

---

### 3. Static Code Analysis – SonarQube

The pipeline performs **code quality and security analysis** using SonarQube.

SonarQube checks for:

* Bugs
* Code smells
* Security vulnerabilities
* Code duplication

If the code passes the quality gate, the pipeline continues.

---

### 4. Dependency Security Scan – Snyk

The project uses **Snyk** to scan third-party dependencies used in the Maven build.

Snyk identifies:

* Vulnerable libraries
* Security risks in dependencies

For demonstration purposes, the pipeline continues even if medium-level vulnerabilities are detected.

---

## Containerization Stage

### 5. Docker Image Build

After the successful build, Jenkins uses a **Dockerfile** to build the application container image.

Docker is installed on the Jenkins server and the **Jenkins user is added to the Docker group** to allow container operations.

Required Jenkins plugins:

* Maven Plugin
* Docker Plugin
* SonarQube Plugin
* Kubernetes Plugin

---

### 6. Container Image Security Scan – Trivy

Before pushing the image to the registry, the image is scanned using **Trivy**.

Trivy detects:

* OS vulnerabilities
* Library vulnerabilities
* Security risks inside the container image

The scan results are visible in the **Jenkins console output**.

---

### 7. Push Image to DockerHub

Once scanning is complete, the Docker image is pushed to **DockerHub** for deployment.

This image is later used by the Kubernetes deployment.

---

## Continuous Deployment (CD) Pipeline

For demonstration purposes, the **CD process is included within the same Jenkins pipeline**.

---

### 8. AWS EKS Cluster Integration

The application is deployed to a **Kubernetes cluster running on AWS EKS**.

Required tools installed on Jenkins server:

* AWS CLI
* kubectl
* eksctl

Jenkins uses stored **AWS credentials** to interact with the cluster.

---

### 9. Kubernetes Deployment

Kubernetes YAML manifests stored in the GitHub repository are used to deploy the application.

The deployment pulls the **Docker image from DockerHub** and creates Kubernetes resources such as:

* Deployment
* Service

The application is exposed using a **NodePort service**.

---

### 10. Kubernetes Security Scan – Trivy

After deployment, **Trivy is used again to scan Kubernetes configuration files**.

This helps detect security issues in:

* Kubernetes YAML manifests
* Kubernetes objects

---

## Slack Notifications using Jenkins Shared Library

To improve **pipeline visibility and team collaboration**, Slack notifications are integrated into the pipeline.

Notifications are triggered when:

* Pipeline starts
* Pipeline succeeds
* Pipeline fails

Instead of placing Slack logic directly inside the Jenkinsfile, the Slack notification code is implemented using a **Jenkins Shared Library**.

Advantages of using Shared Library:

* Reusable pipeline functions
* Cleaner Jenkinsfile
* Centralized notification logic
* Easier pipeline maintenance

The Slack function is called inside the **post section of the Jenkins pipeline** to send notifications automatically.

---

## Application Stack

Backend: Spring Boot
Database: MongoDB
Containerization: Docker
Orchestration: Kubernetes (AWS EKS)

---

## Project Outcome

This project demonstrates a **complete DevSecOps pipeline implementation**, covering:

* Automated CI/CD pipeline design
* Integrated security scanning across multiple stages
* Containerization using Docker
* Kubernetes deployment on AWS EKS
* Automated Slack notifications for pipeline monitoring

It replicates the **real-world DevOps CI/CD workflow used in modern cloud-native organizations**, helping understand how automation, security, and infrastructure work together in production environments.

---

## Project Demonstration & Screenshots

Screenshots of the pipeline execution, security scans, and application deployment are available in the LinkedIn project post below.

LinkedIn Project Post:
www.linkedin.com/in/nitheesh-kumar-devops
## Author

**Nitheesh Kumar Bellamkonda**

DevOps Engineer with hands-on experience in **AWS, Kubernetes, CI/CD pipelines, and cloud-native infrastructure**.

LinkedIn: www.linkedin.com/in/nitheesh-kumar-devops


---

⭐ If you found this project helpful or interesting, feel free to connect with me on LinkedIn.


