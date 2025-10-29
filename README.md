1.1 Project Overview
Overview of Project ☁️
Scenario:

EduTech Solutions is modernizing their outdated LMS by migrating from on-premises to a containerized AWS application. Their initial deployment encountered critical issues preventing proper functionality. As their new Cloud Support Engineer, you'll identify and resolve these containerization issues before semester launch.

Our solution:

A containerized LMS frontend on AWS ECS Fargate with monitoring capabilities. This project demonstrates how AWS container services work together and teaches essential troubleshooting skills for common misconfigurations.

About Project:

We'll recreate EduTech's containerized deployment with the same issues their team faced, then troubleshoot using CloudWatch, ECS logs, and AWS Console. You'll gain hands-on experience diagnosing and fixing real-world container issues just as a cloud support engineer would in a production environment.

Steps to be performed 👩‍💻
In the next few lessons, we'll be going through the following steps.

Setting up the AWS environment for containerized applications
Container image preparation for the LMS frontend
Deploying the LMS frontend on ECS Fargate
Comprehensive troubleshooting of container issues in ECS
Resolving ALB configuration issues
Correcting security group misconfigurations
Proper cleanup of resources
Services Used 🛠
Amazon ECS with Fargate: Serverless compute engine for containers
Application Load Balancer (ALB): Routes HTTP/HTTPS traffic to containers
Amazon CloudWatch: Monitoring and observability for logs and metrics
AWS IAM: Identity and access management for AWS resources
Amazon ECR: Container registry for managing and deploying images
AWS Security Groups: Virtual firewalls controlling network traffic
Estimated Time & Cost ⚙️
This project is estimated to take about 60-90 minutes
Cost: Under $5 USD (with prompt resource cleanup)
➡️ Diagram
This is the architectural diagram for the project:



➡️ Final Result
A fully functional containerized LMS that demonstrates:

Proper container deployment practices
Secure configuration of AWS services
Effective troubleshooting methodologies
Scalable architecture for educational technology solutions
This is what our project will look like, once built:
