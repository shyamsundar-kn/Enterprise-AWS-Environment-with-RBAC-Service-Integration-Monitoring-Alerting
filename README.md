# Production Grade 3-Tier Web Application on AWS

## Project Overview
This project demonstrates the deployment of a **highly available, scalable, and secure Django web application** using AWS cloud services following a **3-tier architecture**.

### Architecture Flow
User → Route 53 → ALB → EC2 (Auto Scaling) → RDS (Multi-AZ)

---

## Key Objectives
- High Availability (Multi-AZ)
- Auto Scaling based on traffic
- Zero Downtime Deployment
- Secure Infrastructure (Private Subnets)
- Centralized Monitoring & Logging

---

## Architecture Diagram
Client (Browser/User)  
→ Route 53 (DNS Resolution)  
→ Application Load Balancer (Public Subnet)  
→ EC2 Instances (Private Subnet - Auto Scaling Group)  
→ Django Application (Gunicorn + Nginx)  
→ Amazon RDS (PostgreSQL - Multi-AZ, Private Subnet)

---

## AWS Services Used

| Service     | Purpose                       |
|------------|------------------------------|
| EC2        | Host Django application      |
| ALB        | Load balancing               |
| ASG        | Auto scaling                 |
| RDS        | Managed database (Multi-AZ)  |
| S3         | Static file storage          |
| IAM        | Secure access                |
| CloudWatch | Monitoring & alerts          |
| Route 53   | DNS routing                  |
| ACM        | SSL certificates             |
| VPC        | Network isolation            |

---

## VPC & Network Setup
- Custom VPC created  
- CIDR Block: `10.0.0.0/16`

### Subnets
- Public Subnet 1 (AZ1)
- Public Subnet 2 (AZ2)
- Private Subnet 1 (AZ1)
- Private Subnet 2 (AZ2)

### Components
- Internet Gateway attached to VPC
- NAT Gateway in Public Subnet

### Route Tables
- Public → Internet Gateway  
- Private → NAT Gateway  

---

## Database Layer (RDS)
- Engine: PostgreSQL / MySQL  
- Multi-AZ enabled  
- Deployed in Private Subnets  

### Security
- No public access  
- Security Group allows only EC2 access (3306 / 5432)  

---

## Application Layer (EC2 + Docker)

### EC2 Configuration
- Docker  
- Docker Compose  
- Nginx (Reverse Proxy)  

### Application Setup
- Django app containerized using Docker  
- Nginx used for reverse proxy & static handling  

---

## AMI Creation
- Preconfigured EC2 with Docker + App  
- Custom AMI used in Auto Scaling  

---

## Launch Template
- AMI ID  
- Instance Type: `t2.micro / t3.small`  
- IAM Role: S3 + CloudWatch  

### User Data
- Pull latest Docker image  
- Start containers  

---

## Auto Scaling Group (ASG)
- Min: 2 | Max: 5 | Desired: 2  

### Scaling Policy
- CPU Utilization: 60%  

---

## Load Balancer (ALB)
- HTTP → HTTPS redirect  
- HTTPS (443) enabled  
- Health checks configured  

---

## SSL Configuration (ACM)
- Certificate created via ACM  
- Validated via Route 53  
- Attached to ALB  

---

## Domain Setup (Route 53)
- Hosted zone created  
- A Record → ALB  

---

## Static Files Handling (S3)

### Django Configuration
```python
STATICFILES_STORAGE = 'storages.backends.s3boto3.S3Boto3Storage'
```
Benefits
* Reduced EC2 load
* Faster content delivery

Monitoring & Logging

CloudWatch
* CPU utilization
* Network traffic
* Application logs
* System logs
  
Alerts
* Trigger when CPU > 60%
* SNS notifications enabled


High Availability Design
* Multi-AZ deployment
* ALB + ASG across AZs
* RDS Multi-AZ


Zero Downtime Deployment
* Rolling updates via ASG
* Health checks before traffic routing


Problems Faced & Solutions

SSL Validation Issue
* Fixed DNS validation
  
Static Files 403
* Fixed S3 permissions

RDS Connectivity
* Fixed Security Group rules

Deployment Downtime
* Implemented rolling updates

Scaling Delay
* Configured auto scaling

Key Solutions Implemented
* 3-tier AWS architecture
* Auto Scaling + Load Balancer
* Multi-AZ database
* CloudWatch + SNS integration
* Secure infrastructure
* Dockerized application

Business Impact
* High availability → minimal downtime
* Auto scaling → better performance
* Secure architecture → reduced risk
* Cost optimization
* Faster deployments

PHASE 1 — IAM Setup

Roles: Admin, Developer, Auditor, DBAdmin, Support
MFA enabled

PHASE 2 — S3 Logging

company-data & audit-logs buckets
Access logging enabled

PHASE 3 — EC2 Setup

Node.js app deployed
Security groups configured

PHASE 4 — Lambda Integration

Trigger: S3 upload
Logs → CloudWatch

PHASE 5 — Monitoring

CloudWatch Agent installed
CPU, Memory, Disk metrics

PHASE 6 — Alarms

CPU > 70%
Memory > 80%
Disk > 80%

PHASE 7 — SNS Alerts

Email notifications configured

PHASE 8 — Testing

Stress testing performed
Alerts verified

PHASE 9 — RDS

Private database
Access via EC2

PHASE 10 — Security

CloudTrail + AWS Config enabled

PHASE 11 — Dashboard

CloudWatch dashboard
Optional Grafana
