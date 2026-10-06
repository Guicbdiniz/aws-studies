# ☁️ AWS Studies

A hands-on learning repository for mastering core AWS services through practical projects. Each project is designed to build real-world skills while covering one or more key services.

---

## 🗺️ Learning Roadmap

The projects are organized into **three phases** of increasing complexity:

| Phase | Focus | Difficulty |
|-------|-------|------------|
| 🟢 Foundation | Core services in isolation | Beginner |
| 🟡 Integration | Multi-service architectures | Intermediate |
| 🔴 Production | Full-stack, production-grade systems | Advanced |

---

## 🟢 Phase 1 — Foundation Projects

### 📁 Project 1: Static Website with CloudFront & S3
> **Services:** S3, CloudFront, Route 53

Deploy a static website (portfolio or landing page) to an S3 bucket, distribute it globally with CloudFront, and point a custom domain to it using Route 53.

**Key learnings:**
- S3 bucket policies and static website hosting
- CloudFront distributions, cache behaviors, and invalidations
- Route 53 hosted zones and DNS record types (A, CNAME, Alias)
- HTTPS with ACM (SSL/TLS certificates)

---

### 🔑 Project 2: IAM Deep Dive — Secure Access Management
> **Services:** IAM

Create a realistic IAM setup for a fictional company with multiple teams (Devs, Ops, Finance), following the principle of least privilege.

**Key learnings:**
- IAM users, groups, roles, and policies
- Inline vs. managed policies
- Cross-account roles and role assumption
- IAM policy simulator and access analyzer
- MFA enforcement and password policies

---

### ⚡ Project 3: Serverless URL Shortener
> **Services:** Lambda, DynamoDB, S3, CloudFront

Build a URL shortener API using Lambda functions with DynamoDB as the data store. The frontend is hosted on S3 + CloudFront.

**Key learnings:**
- Writing and deploying Lambda functions (Node.js or Python)
- DynamoDB tables, primary keys, and basic CRUD operations
- Lambda function URLs or API Gateway integration
- Environment variables and Lambda execution roles (IAM)

---

### 🖥️ Project 4: Web Server on EC2
> **Services:** EC2, Amazon VPC, IAM, CloudWatch

Manually provision a web server on EC2 inside a custom VPC. Configure security groups, key pairs, and set up basic monitoring.

**Key learnings:**
- EC2 instance types, AMIs, and user data scripts
- VPC fundamentals: subnets (public/private), Internet Gateway, route tables
- Security groups vs. NACLs
- CloudWatch metrics, alarms, and basic dashboards
- IAM instance profiles and roles

---

## 🟡 Phase 2 — Integration Projects

### 🛒 Project 5: Serverless E-Commerce API
> **Services:** Lambda, DynamoDB, S3, CloudFront, IAM, CloudWatch

Build a REST API for an e-commerce store: product catalog, cart, and order management. All serverless, with observability built in.

**Key learnings:**
- API Gateway + Lambda integrations (proxy and non-proxy)
- DynamoDB single-table design and GSIs (Global Secondary Indexes)
- S3 pre-signed URLs for product image uploads
- CloudWatch Logs Insights and Lambda metrics
- Structured logging and X-Ray tracing

---

### 🐳 Project 6: Containerized Microservice on ECS
> **Services:** ECS, EC2, Amazon VPC, IAM, CloudWatch, S3

Containerize a simple microservice (e.g., a notes API) and deploy it to ECS using Fargate. Store assets in S3 and monitor with CloudWatch.

**Key learnings:**
- Docker containerization and ECR (Elastic Container Registry)
- ECS clusters, task definitions, and services (Fargate vs. EC2 launch type)
- Application Load Balancer (ALB) with ECS
- VPC networking for containers (ENIs, security groups)
- ECS task IAM roles and CloudWatch Container Insights

---

### 🗄️ Project 7: Multi-Tier App with RDS
> **Services:** EC2, RDS, Amazon VPC, IAM, CloudWatch

Deploy a classic 3-tier web application: EC2 (web + app tier) backed by a managed relational database on RDS (PostgreSQL or MySQL) inside a private subnet.

**Key learnings:**
- RDS instance setup, storage, and parameter groups
- VPC design: public vs. private subnets, NAT Gateway
- RDS Multi-AZ for high availability
- RDS automated backups and snapshots
- CloudWatch RDS metrics and Enhanced Monitoring

---

## 🔴 Phase 3 — Production Projects

### 🌐 Project 8: Highly Available Web Platform
> **Services:** EC2, Amazon VPC, RDS, Aurora, CloudFront, Route 53, IAM, CloudWatch

Build a production-grade, highly available web platform with auto-scaling EC2 instances, an Aurora database cluster, and a global CDN layer.

**Key learnings:**
- EC2 Auto Scaling Groups with launch templates
- Application Load Balancer across multiple AZs
- Aurora cluster (writer + reader instances) and failover behavior
- Aurora vs. RDS trade-offs (serverless, global database)
- Route 53 health checks and failover routing policies
- CloudWatch dashboards, composite alarms, and SNS notifications

---

### 📊 Project 9: Serverless Data Pipeline & Analytics Dashboard
> **Services:** Lambda, S3, DynamoDB, CloudWatch, IAM

Build an event-driven data pipeline that ingests data (e.g., simulated IoT sensor readings or logs), processes it with Lambda, stores raw data in S3 and aggregated results in DynamoDB, and visualizes metrics in CloudWatch dashboards.

**Key learnings:**
- S3 event notifications triggering Lambda
- DynamoDB Streams and Lambda triggers
- S3 lifecycle policies and storage tiers (Standard, IA, Glacier)
- CloudWatch custom metrics and metric math
- EventBridge (CloudWatch Events) for scheduled jobs

---

### 🏗️ Project 10: Full Cloud-Native Platform (Capstone)
> **Services:** EC2, Lambda, ECS, S3, RDS, DynamoDB, Aurora, Amazon VPC, CloudFront, Route 53, IAM, CloudWatch

The capstone project ties everything together: a real-world SaaS-like application with a frontend, multiple backend services (some containerized, some serverless), relational and NoSQL databases, a CDN, custom domain, and full observability.

**Key learnings:**
- Architecting for the AWS Well-Architected Framework (Reliability, Security, Performance, Cost)
- Infrastructure as Code with AWS CloudFormation or Terraform
- Blue/green and canary deployments
- Cost optimization strategies (right-sizing, Reserved/Spot instances)
- Security best practices (VPC flow logs, GuardDuty, Config)

---

## 📋 Services Coverage Matrix

| Service | P1 | P2 | P3 | P4 | P5 | P6 | P7 | P8 | P9 | P10 |
|---|---|---|---|---|---|---|---|---|---|---|
| **S3** | ✅ | | | | ✅ | ✅ | | | ✅ | ✅ |
| **CloudFront** | ✅ | | | | ✅ | | | ✅ | | ✅ |
| **Route 53** | ✅ | | | | | | | ✅ | | ✅ |
| **IAM** | | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| **Lambda** | | | ✅ | | ✅ | | | | ✅ | ✅ |
| **DynamoDB** | | | ✅ | | ✅ | | | | ✅ | ✅ |
| **EC2** | | | | ✅ | | | ✅ | ✅ | | ✅ |
| **Amazon VPC** | | | | ✅ | | ✅ | ✅ | ✅ | | ✅ |
| **CloudWatch** | | | | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| **ECS** | | | | | | ✅ | | | | ✅ |
| **RDS** | | | | | | | ✅ | ✅ | | ✅ |
| **Aurora** | | | | | | | | ✅ | | ✅ |

---

## 📂 Repository Structure

```
aws-studies/
├── README.md
└── projects/
    ├── project-01-static-website/   🟢 Phase 1
    ├── project-02-iam-deep-dive/    🟢 Phase 1
    ├── project-03-url-shortener/    🟢 Phase 1
    ├── project-04-ec2-web-server/   🟢 Phase 1
    ├── project-05-ecommerce-api/    🟡 Phase 2
    ├── project-06-ecs-microservice/ 🟡 Phase 2
    ├── project-07-rds-multi-tier/   🟡 Phase 2
    ├── project-08-ha-web-platform/  🔴 Phase 3
    ├── project-09-data-pipeline/    🔴 Phase 3
    └── project-10-capstone/         🔴 Phase 3
```

Each project folder will contain:
- `README.md` — Architecture, setup steps, and key learnings
- `src/` — Application source code
- `infra/` — CloudFormation / Terraform templates
- `docs/` — Architecture diagrams and notes

---

## 🛠️ Prerequisites

- AWS Account (Free Tier eligible where possible)
- AWS CLI configured (`aws configure`)
- Node.js / Python (for Lambda functions)
- Docker (for ECS projects)
- Terraform or AWS CDK (optional, for IaC)

---

## 📚 Useful Resources

- [AWS Documentation](https://docs.aws.amazon.com)
- [AWS Free Tier](https://aws.amazon.com/free/)
- [AWS Well-Architected Framework](https://aws.amazon.com/architecture/well-architected/)
- [AWS Architecture Center](https://aws.amazon.com/architecture/)

---

> 💡 **Tip:** Work through the projects in order — each one builds on skills from the previous phase. Don't skip the Foundation phase even if you're already familiar with some services!
