# ⚡ Project 03 — Serverless URL Shortener

## 🎯 Goal

Build a fully serverless URL shortener (think bit.ly) using Lambda for logic, DynamoDB for storage, and S3 + CloudFront for the frontend. No servers to manage.

## 🏗️ Architecture

```
Browser
  │
  ├─► CloudFront ──► S3 (frontend HTML/JS)
  │
  └─► Lambda Function URL (or API Gateway)
        │
        ├─► POST /shorten  ──► DynamoDB (write short code)
        └─► GET  /{code}   ──► DynamoDB (read & redirect)
```

## 🛠️ AWS Services

| Service | Role |
|---------|------|
| **Lambda** | Shorten URL logic + redirect handler |
| **DynamoDB** | Store `{ shortCode → originalUrl, createdAt, clicks }` |
| **S3** | Host the frontend (form to submit URLs) |
| **CloudFront** | Serve frontend + route API calls |
| **IAM** | Lambda execution role with DynamoDB permissions |

## 📋 Step-by-Step Plan

### Step 1 — Design the Data Model (DynamoDB)
- [ ] Table name: `url-shortener`
- [ ] Partition key: `shortCode` (String)
- [ ] Attributes: `originalUrl`, `createdAt`, `clicks`, `expiresAt` (TTL)
- [ ] Enable TTL on `expiresAt` for auto-expiring links

### Step 2 — Write the Lambda Functions

#### `POST /shorten`
- [ ] Accept `{ "url": "https://..." }` in the request body
- [ ] Validate the URL format
- [ ] Generate a unique short code (e.g., 6-char nanoid)
- [ ] Write to DynamoDB
- [ ] Return `{ "shortUrl": "https://short.ly/abc123" }`

#### `GET /{code}`
- [ ] Look up `shortCode` in DynamoDB
- [ ] If found: return HTTP 301/302 redirect to `originalUrl`, increment `clicks`
- [ ] If not found: return 404

### Step 3 — Set Up IAM Role for Lambda
- [ ] Create execution role with:
  - `dynamodb:GetItem`, `dynamodb:PutItem`, `dynamodb:UpdateItem`
  - `logs:CreateLogGroup`, `logs:CreateLogStream`, `logs:PutLogEvents`

### Step 4 — Deploy Lambda
- [ ] Use **Lambda Function URLs** (simplest) or API Gateway (more control)
- [ ] Set runtime: Python 3.12 or Node.js 20
- [ ] Configure environment variables: `TABLE_NAME`, `BASE_URL`
- [ ] Test via the AWS Console and `curl`

### Step 5 — Build the Frontend
- [ ] Simple HTML page with a form (URL input + submit button)
- [ ] Vanilla JS: calls the Lambda Function URL, displays the short link
- [ ] Deploy to S3 + CloudFront (reuse knowledge from Project 01!)

### Step 6 — Wire Everything Together
- [ ] Update CloudFront to route `GET /{code}` to Lambda and everything else to S3
- [ ] Or use a standalone CloudFront function for redirects

### Step 7 — Observability (Bonus)
- [ ] Add CloudWatch metrics for Lambda invocations, errors, duration
- [ ] Create a CloudWatch dashboard: total links created, total clicks, error rate
- [ ] Set up an alarm if error rate exceeds 5%

## 📁 Folder Structure

```
project-03-url-shortener/
├── README.md
├── src/
│   ├── shorten/
│   │   ├── handler.py (or index.js)
│   │   └── requirements.txt
│   ├── redirect/
│   │   └── handler.py (or index.js)
│   └── frontend/
│       ├── index.html
│       └── app.js
└── infra/
    ├── dynamodb-table.json
    ├── lambda-role-policy.json
    └── cloudformation/
        └── url-shortener-stack.yaml
```

## 🔑 Key Concepts to Understand

- **Lambda cold starts** — what they are and how to mitigate (provisioned concurrency)
- **DynamoDB capacity modes** — On-Demand vs Provisioned (use On-Demand for this project)
- **TTL in DynamoDB** — automatic item expiration without extra cost
- **Lambda Function URLs** vs **API Gateway** — trade-offs
- **Lambda execution role** — why Lambda needs an IAM role to access other services
- **Atomic counters in DynamoDB** — using `UpdateExpression: ADD clicks :1`

## 💰 Estimated Cost

| Service | Free Tier | Notes |
|---------|-----------|-------|
| Lambda | 1M requests/month | Essentially free for this project |
| DynamoDB | 25 GB + 25 WCU/RCU | On-Demand is cheaper for low traffic |
| S3 + CloudFront | Covered by Project 01 tier | — |

## ✅ Done When...

- [ ] `POST /shorten` with a URL returns a working short link
- [ ] Visiting the short link redirects to the original URL
- [ ] Click count increments on each visit
- [ ] Links expire automatically after 30 days
- [ ] A simple frontend lets you shorten URLs in the browser
