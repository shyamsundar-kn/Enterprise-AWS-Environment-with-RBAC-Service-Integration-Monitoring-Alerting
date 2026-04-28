# Enterprise AWS Environment with RBAC, Service Integration, Monitoring & Alerting

## Project Overview
This project demonstrates the design and implementation of a **secure, scalable, and observable AWS cloud environment** integrating:

- Role-Based Access Control (RBAC)
- Multi-service architecture
- Real-time monitoring with automated alerting

It simulates an enterprise-grade infrastructure where multiple teams interact securely with AWS services while maintaining full operational visibility.

### Core Capabilities
- Infrastructure provisioning  
- Service integration  
- Security enforcement  
- Monitoring and alerting  

### AWS Services Used
- IAM (Identity and Access Management)
- EC2
- S3
- Lambda
- RDS
- CloudWatch
- SNS
- CloudTrail
- Athena  

---

## Objectives
- Implement secure multi-user access using IAM  
- Deploy a multi-tier architecture  
- Enable event-driven processing (Lambda)  
- Monitor infrastructure using CloudWatch Agent  
- Configure automated alerts via SNS  
- Enforce least privilege access  
- Enable audit logging and analysis  
- Perform end-to-end validation  

---

## Architecture Overview

### High-Level Workflow
1. Users access AWS via IAM roles  
2. Application runs on EC2  
3. Data stored in S3 & RDS  
4. S3 events trigger Lambda  
5. Lambda logs → CloudWatch  
6. CloudTrail logs → S3  
7. Athena queries logs  
8. CloudWatch Agent monitors EC2  
9. CloudWatch Alarms → SNS notifications  

---

## Core Services

| Service      | Purpose                      |
|-------------|------------------------------|
| IAM         | Authentication & RBAC        |
| EC2         | Application hosting          |
| S3          | Storage & logging            |
| Lambda      | Event-driven processing      |
| RDS         | Database                    |
| CloudWatch  | Monitoring & observability   |
| SNS         | Alert notifications          |
| CloudTrail  | Audit logging                |
| Athena      | Log analysis                 |

---

## IAM & Role-Based Access Control

### Roles Defined

| Role         | Permissions                  |
|-------------|------------------------------|
| Admin       | Full access                  |
| Developer   | EC2, S3, Lambda              |
| Auditor     | Read-only logs               |
| DBAdmin     | Full RDS access              |
| SupportTeam | EC2 start/stop               |

### Implementation
- IAM groups created per role  
- Managed + custom policies attached  
- Least privilege enforced  
- MFA enabled for Admins  
- IAM Access Analyzer used  

---

## Storage & Logging (S3)

### Buckets
- `company-data` → Application data  
- `audit-logs` → Log storage  

### Configuration
- Server access logging enabled  
- Logs stored in audit bucket  
- Access control:
  - Auditor → Read-only  
  - Others → Restricted  

---

## Compute Layer (EC2)

### Configuration
- AMI: Ubuntu / Amazon Linux 2  
- Instance: t2.medium  
- Ports:
  - 22 → SSH  
  - 80 → HTTP  

### Features
- Node.js app deployed  
- IAM role attached for S3  
- Resource tagging implemented  

---

## Serverless Layer (Lambda)

### Implementation
- Triggered by S3 upload events  
- Processes files (logs/images)  

### Permissions
- S3 read/write  
- CloudWatch logging  

### Access Control
- Developers → Invoke-only  

---

## Database Layer (RDS)

### Configuration
- Engine: MySQL  
- Automated backups enabled  
- Secured via security groups  

### Access Control
- Only DBAdmin role allowed  
- Policy restricted via ARN  

---

## Monitoring & Alerting

### CloudWatch Agent
- Installed on EC2  
- Metrics collected:
  - CPU  
  - Memory  
  - Disk  

### Metrics Namespace
- `CWAgent`

### CloudWatch Alarms

| Metric           | Threshold |
|----------------|----------|
| CPU Utilization | > 70%    |
| Memory Usage    | > 75%    |
| Disk Usage      | > 80%    |

### SNS Notifications
- Topic created  
- Email subscription added  
- Alerts triggered on threshold breach  

### Dashboard
- EC2 metrics  
- Lambda metrics  
- Alarm status  
- System health overview  

---

## Cross-Service Integration

| Source       | Target      | Purpose          |
|-------------|------------|------------------|
| S3          | Lambda     | Event trigger     |
| Lambda      | CloudWatch | Logging           |
| EC2         | S3         | Data access       |
| CloudTrail  | S3         | Audit storage     |
| S3          | Athena     | Log analysis      |
| CloudWatch  | SNS        | Alerts            |

---

## Security & Compliance
- MFA enabled  
- Least privilege access  
- IAM Access Analyzer  
- AWS Config enabled  
- CloudTrail enabled  
- Secure security groups  

---

## Logging & Audit Analysis

### CloudTrail
Tracks:
- API calls  
- User actions  
- Resource changes  

### Athena
- Queries logs stored in S3  
- Enables audit & compliance reporting  

---

## Testing & Validation

### IAM Testing
- Verified role-based access  
- Tested allowed/denied actions  

### Functional Testing
- EC2 app accessible  
- S3 upload successful  
- Lambda triggered  
- Logs visible in CloudWatch  
- RDS connectivity verified  

### Monitoring Validation
- Simulated CPU load  
- Verified alarms  
- SNS alerts confirmed  

### Integration Testing
- S3 → Lambda → CloudWatch  
- CloudTrail → S3 → Athena  

---

## Challenges Faced
- IAM policy conflicts  
- Lambda trigger misconfiguration  
- Delay in S3 logging  
- CloudWatch Agent complexity  
- Security group issues  

---

## Key Learnings
- Deep understanding of IAM & RBAC  
- Real-world AWS architecture design  
- Importance of observability  
- Hands-on service integration  
- Security-first implementation  

---

## Future Enhancements
- CI/CD with CodePipeline  
- API Gateway integration  
- Terraform (IaC)  
- Grafana dashboards  
- EC2 Auto Scaling  
- OpenSearch for centralized logging  

---

## Conclusion
This project demonstrates a **production-grade AWS architecture** with:

- Secure access control (IAM + RBAC)  
- Multi-service integration  
- Real-time monitoring & alerting  
- Logging and auditing  

It is **scalable, secure, and highly aligned with real-world enterprise cloud environments**, making it ideal for DevOps and Cloud Engineering roles.

---
