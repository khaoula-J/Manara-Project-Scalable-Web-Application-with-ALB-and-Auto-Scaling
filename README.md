# Manara-Project-Scalable-Web-Application-with-ALB-and-Auto-Scaling
This repository demonstrates a **highly available, secure, and auto-scalable** web application deployed on AWS using core services such as **EC2, Application Load Balancer (ALB), Auto Scaling Groups (ASG), Amazon RDS, and ElastiCache**.

The architecture follows AWS best practices for **security, performance, observability, and cost optimization**, making it suitable for production workloads.

<img width="2900" height="1680" alt="Web application achitecture  (1)" src="https://github.com/user-attachments/assets/684b39f3-202f-4db5-bac4-6d1fd7cf083c" />

🖼️ Architecture Overview


The application is deployed across a **multi-AZ VPC** to ensure high availability. Internet traffic is routed through **Route 53** and an **Application Load Balancer (ALB)**, which distributes requests to EC2 instances running in private subnets. The backend database (RDS) and caching layer (ElastiCache) are also hosted in private subnets for enhanced security.

Auto Scaling ensures the application can handle variable loads, while CloudWatch and SNS provide monitoring and alerting.

 🔧 Key Components

| Service | Role |
|-------|------|
| **Amazon VPC** | Isolated network with public and private subnets across two Availability Zones (AZ-A and AZ-B). |
| **Route 53** | DNS service that routes user traffic to the ALB via a domain name (e.g., `app.example.com`). |
| **Application Load Balancer (ALB)** | Distributes HTTPS traffic (port 443) across EC2 instances. SSL/TLS termination is handled at the ALB. |
| **Auto Scaling Group (ASG)** | Maintains a minimum of 2 EC2 instances and scales up to 10 based on CPU utilization (e.g., scale out when CPU > 70%). |
| **EC2 Instances** | Host the web application (e.g., Apache/Nginx + app code). Launched from a Launch Template and placed in private subnets. |
| **Amazon RDS (MySQL/PostgreSQL)** | Managed relational database with Multi-AZ deployment for high availability and automatic failover. |
| **Amazon ElastiCache (Redis)** | In-memory cache for frequently accessed data (e.g., sessions, product info), reducing database load. |
| **NAT Gateway** | Enables outbound internet access for EC2 instances in private subnets (e.g., for updates, logs). |
| **CloudWatch & SNS** | Monitors CPU, latency, and request count. Sends alerts via email/SMS when thresholds are breached. |
| **IAM Roles** | Grants EC2 instances secure access to AWS services (e.g., CloudWatch, SSM) without hardcoded credentials. |
| **Security Groups** | Act as virtual firewalls to control inbound and outbound traffic at the instance level. |



 🔐 Security Features

- ✅ All application and database resources are in **private subnets**.
- ✅ **No public IPs** on EC2 or RDS instances.
- ✅ **Security Groups** enforce least-privilege access:
  - ALB: Allows HTTPS (443) from `0.0.0.0/0`
  - EC2: Allows HTTP (80) only from ALB
  - RDS: Allows MySQL/PostgreSQL only from EC2 SG
  - ElastiCache: Allows Redis only from EC2 SG
- ✅ **NAT Gateway** in public subnets allows secure outbound access.
- ✅ TLS termination at ALB using certificates from **AWS Certificate Manager (ACM)**.

 🚀 Scalability & High Availability

- Auto Scaling Group dynamically adds or removes EC2 instances based on CPU usage.
- ALB ensures even traffic distribution and performs health checks.
- RDS Multi-AZ provides automatic failover during outages.
- ElastiCache improves performance by reducing database read load.



 📈 Monitoring & Alerts

- CloudWatch collects metrics from EC2, ALB, and RDS.
- Alarms trigger scaling actions or notify via **SNS** (email/SMS).
- Example alarm: `CPUUtilization > 70% for 5 minutes → Scale Out`

