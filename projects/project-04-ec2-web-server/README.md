# 🖥️ Project 04 — Web Server on EC2 with VPC & CloudWatch

## 🎯 Goal

Manually provision a production-like environment from scratch: a custom VPC with public and private subnets, an EC2 instance running a web server, and CloudWatch for monitoring. This project is intentionally "hands-on" with no IaC — learn the console and CLI first.

## 🏗️ Architecture

```
Internet
   │
   ▼
Internet Gateway
   │
   ▼
Public Subnet (10.0.1.0/24)
   ├── EC2 Web Server (t3.micro)
   │     └── Nginx / Node.js app
   └── Bastion Host (optional, t3.nano)

Private Subnet (10.0.2.0/24)
   └── (reserved for Project 07 — RDS)

CloudWatch
   └── EC2 metrics, alarms, dashboard
```

## 🛠️ AWS Services

| Service | Role |
|---------|------|
| **EC2** | Virtual machine running the web server |
| **Amazon VPC** | Isolated network with public/private subnets |
| **IAM** | Instance profile for CloudWatch agent |
| **CloudWatch** | Metrics, alarms, and dashboard |

## 📋 Step-by-Step Plan

### Step 1 — Design the VPC
- [ ] Create a VPC: CIDR `10.0.0.0/16`
- [ ] Create subnets:
  - Public: `10.0.1.0/24` (AZ: us-east-1a)
  - Private: `10.0.2.0/24` (AZ: us-east-1b)
- [ ] Create and attach an **Internet Gateway**
- [ ] Update the **public route table**: `0.0.0.0/0 → igw-xxx`
- [ ] Enable **auto-assign public IPv4** on the public subnet

### Step 2 — Configure Security Groups
- [ ] **Web Server SG:**
  - Inbound: `80` (HTTP) from `0.0.0.0/0`, `443` (HTTPS) from `0.0.0.0/0`
  - Inbound: `22` (SSH) from your IP only
  - Outbound: all traffic
- [ ] Understand the difference between Security Groups (stateful) and NACLs (stateless)

### Step 3 — Launch the EC2 Instance
- [ ] AMI: Amazon Linux 2023 (free tier eligible)
- [ ] Instance type: `t3.micro` (or `t2.micro` for free tier)
- [ ] Place in the **public subnet**
- [ ] Attach the **Web Server SG**
- [ ] Create a key pair and download the `.pem` file
- [ ] Add **User Data** script to install and start Nginx on boot:

```bash
#!/bin/bash
dnf update -y
dnf install -y nginx
systemctl start nginx
systemctl enable nginx
echo "<h1>Hello from EC2!</h1>" > /usr/share/nginx/html/index.html
```

### Step 4 — Create an IAM Instance Profile
- [ ] Create a role: `ec2-cloudwatch-role`
- [ ] Attach policy: `CloudWatchAgentServerPolicy`
- [ ] Attach the instance profile to the EC2 instance

### Step 5 — Install & Configure CloudWatch Agent
- [ ] SSH into the instance: `ssh -i key.pem ec2-user@<public-ip>`
- [ ] Install the CloudWatch agent:
  ```bash
  dnf install -y amazon-cloudwatch-agent
  ```
- [ ] Run the configuration wizard: `/opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-config-wizard`
- [ ] Collect: memory, disk usage, CPU (basic EC2 metrics don't include memory!)
- [ ] Start the agent and verify metrics appear in CloudWatch

### Step 6 — Create CloudWatch Alarms
- [ ] **High CPU alarm:** CPU > 80% for 5 minutes → notify via SNS email
- [ ] **Status check alarm:** instance fails status check → notify via SNS
- [ ] Create an SNS topic and subscribe your email

### Step 7 — Create a CloudWatch Dashboard
- [ ] Add widgets: CPU utilization, network in/out, disk I/O, memory (from agent)
- [ ] Add a text widget documenting the instance details

### Step 8 — Explore & Experiment
- [ ] Simulate high CPU: `stress --cpu 4 --timeout 60` (install `stress` tool)
- [ ] Watch the alarm trigger in real time
- [ ] Try stopping and starting the instance — notice the public IP changes
- [ ] Assign an **Elastic IP** to keep a static public IP

### Step 9 — Cleanup Script (Important!)
- [ ] Stop the EC2 instance when not studying to avoid charges
- [ ] Or terminate and recreate from User Data (practice makes perfect!)

## 📁 Folder Structure

```
project-04-ec2-web-server/
├── README.md
├── scripts/
│   ├── user-data.sh          # EC2 bootstrap script
│   ├── cloudwatch-config.json # CloudWatch agent config
│   └── stress-test.sh        # CPU stress test script
└── infra/
    ├── security-groups.md    # SG rules documented
    ├── vpc-design.md         # Subnet CIDR plan
    └── iam-instance-profile.json
```

## 🔑 Key Concepts to Understand

- **VPC CIDR blocks** — how IP ranges are allocated and why subnets must be subsets
- **Public vs Private subnets** — a subnet is "public" only if it has a route to an IGW
- **NAT Gateway** — allows private subnet instances to reach the internet (outbound only)
- **Security Groups are stateful** — return traffic is automatically allowed
- **Instance metadata service** — `http://169.254.169.254/latest/meta-data/` from inside EC2
- **EC2 User Data** — runs once at first boot as root
- **CloudWatch default metrics** — EC2 sends CPU, network, disk I/O but NOT memory
- **Custom metrics** — require the CloudWatch agent or manual `put-metric-data` API calls

## 💰 Estimated Cost

| Service | Free Tier | Notes |
|---------|-----------|-------|
| EC2 t2.micro | 750 hrs/month | **Stop when not in use!** |
| CloudWatch | 10 custom metrics, 10 alarms | Free for basic metrics |
| Elastic IP | Free when attached | \$0.005/hr when unattached |
| NAT Gateway | **Not free** | Skip for this project — use public subnet |

## ✅ Done When...

- [ ] Custom VPC with public/private subnets is created from scratch
- [ ] EC2 web server is reachable via its public IP on port 80
- [ ] Memory and disk metrics appear in CloudWatch (via agent)
- [ ] A CPU alarm triggers and sends an email notification
- [ ] A CloudWatch dashboard visualizes all key metrics
