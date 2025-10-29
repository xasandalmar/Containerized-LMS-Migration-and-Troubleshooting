# Project 1: Containerized LMS Migration and Troubleshooting

This project demonstrates how to **migrate a Learning Management System (LMS)** into a **containerized environment** using **AWS ECS Fargate**, and how to **troubleshoot** real-world issues related to containers, ALB configurations, and security groups.

---

## 📋 Table of Contents
1. [Project Overview](docs/01-overview.md)
2. [Setting up the AWS Environment](docs/02-aws-setup.md)
3. [Container Image Preparation](docs/03-container-image-prep.md)
4. [Deploying LMS Frontend on ECS Fargate](docs/04-deploy-ecs-fargate.md)
5. [Troubleshooting ECS Containers](docs/05-troubleshooting-ecs.md)
6. [Fixing ALB Configuration Issues](docs/06-alb-issues.md)
7. [Security Group Lab](docs/07-security-group-lab.md)
8. [Conclusion and Resource Cleanup](docs/08-cleanup.md)

---

## 🏗️ Architecture Diagram
![Architecture Diagram](docs/architecture-diagram.png)

The LMS frontend runs as a containerized application on **AWS ECS Fargate**, fronted by an **Application Load Balancer (ALB)** within a secure **VPC** network. Logs and metrics are collected via **CloudWatch**.

---

## 🚀 Technologies Used
- AWS ECS (Fargate)
- AWS ECR
- AWS ALB + VPC + Security Groups
- Docker
- Terraform (for infrastructure)
- CloudWatch (for monitoring)

---

## ⚙️ Quick Start
```bash
# Clone repo
git clone https://github.com/hassandalmar/lms-containerized-migration.git
cd lms-containerized-migration

# Build image
bash scripts/build-image.sh

# Push to AWS ECR
bash scripts/push-image.sh

# Deploy infrastructure
cd infrastructure/terraform
terraform apply
