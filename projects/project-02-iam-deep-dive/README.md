# 🔑 Project 02 — IAM Deep Dive: Secure Access Management

## 🎯 Goal

Set up a realistic IAM structure for a fictional company called **AcmeCorp** with multiple teams. Practice the principle of least privilege, role assumption, and policy writing — without creating any actual application infrastructure.

## 🏗️ Scenario

**AcmeCorp** has four teams, each needing different levels of AWS access:

| Team | Access Needed |
|------|--------------|
| `developers` | Deploy Lambda, read/write S3 (dev bucket only) |
| `ops` | Full EC2, read CloudWatch, manage VPC |
| `finance` | Read-only access to Cost Explorer & Billing |
| `admins` | Full IAM and account management |

## 🛠️ AWS Services

| Service | Role |
|---------|------|
| **IAM** | Users, groups, roles, and policies |

## 📋 Step-by-Step Plan

### Step 1 — Account Baseline Security
- [ ] Enable MFA on the root account
- [ ] Create a strong account password policy (min length, complexity, rotation)
- [ ] Create a billing alarm (CloudWatch + SNS) to avoid surprises

### Step 2 — Create IAM Groups & Policies
- [ ] Create groups: `developers`, `ops`, `finance`, `admins`
- [ ] Write **custom managed policies** for each group (JSON)
  - Developers: `lambda:*`, `s3:*` scoped to `arn:aws:s3:::acmecorp-dev-*`
  - Ops: `ec2:*`, `cloudwatch:Get*`, `cloudwatch:List*`, `vpc:*`
  - Finance: `ce:*`, `aws-portal:View*` (read-only billing)
  - Admins: `iam:*`

### Step 3 — Create IAM Users
- [ ] Create users: `alice` (dev), `bob` (ops), `carol` (finance), `dan` (admin)
- [ ] Assign each to the appropriate group
- [ ] Enable MFA for all users
- [ ] Generate access keys for `alice` and `bob` (they need CLI access)

### Step 4 — Create IAM Roles
- [ ] **`dev-deploy-role`** — assumed by developers to deploy to staging/prod
- [ ] **`ec2-s3-read-role`** — instance profile for EC2 to read from S3
- [ ] **`cross-account-read-role`** — allows a "partner" account to read S3 (simulate with a second IAM user)

### Step 5 — Test with the Policy Simulator
- [ ] Use the **IAM Policy Simulator** to verify each group's permissions
- [ ] Try to perform actions that should be denied — confirm they are
- [ ] Use `aws iam simulate-principal-policy` from the CLI

### Step 6 — Explore IAM Access Analyzer
- [ ] Enable **IAM Access Analyzer** for the account
- [ ] Review any findings (e.g., publicly accessible S3 buckets, shared roles)
- [ ] Understand the difference between resource-based and identity-based policies

### Step 7 — Document with Policy as Code (Bonus)
- [ ] Export all custom policies to JSON files in `infra/policies/`
- [ ] Write a CloudFormation template that creates the full IAM structure

## 📁 Folder Structure

```
project-02-iam-deep-dive/
├── README.md
└── infra/
    ├── policies/
    │   ├── developer-policy.json
    │   ├── ops-policy.json
    │   ├── finance-policy.json
    │   └── admin-policy.json
    ├── roles/
    │   ├── dev-deploy-role.json
    │   ├── ec2-s3-read-role.json
    │   └── cross-account-read-role.json
    └── cloudformation/
        └── iam-stack.yaml
```

## 🔑 Key Concepts to Understand

- **Identity-based vs resource-based policies** — who can do what vs who can access this
- **Permission boundaries** — capping the maximum permissions a role can have
- **Trust policy** — who (principal) can assume a role
- **Condition keys** — `aws:MultiFactorAuthPresent`, `aws:RequestedRegion`, `aws:SourceIp`
- **`NotAction` / `NotResource`** — powerful but dangerous — understand the difference
- **Service Control Policies (SCPs)** — org-level guardrails (if using AWS Organizations)

## 📝 Sample Policy to Write

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowS3DevBucketsOnly",
      "Effect": "Allow",
      "Action": ["s3:GetObject", "s3:PutObject", "s3:DeleteObject"],
      "Resource": "arn:aws:s3:::acmecorp-dev-*/*",
      "Condition": {
        "Bool": { "aws:MultiFactorAuthPresent": "true" }
      }
    }
  ]
}
```

## ✅ Done When...

- [ ] Each user can only perform actions their group policy allows
- [ ] All policies are stored as JSON in the repo
- [ ] IAM Access Analyzer shows no unintended public access
- [ ] A CloudFormation template can recreate the entire IAM setup from scratch
