# AWS Interview Preparation Guide

This guide contains the most commonly asked AWS interview questions, along with detailed explanations and real-world examples. It covers IAM, S3, VPC, CloudWatch, CloudTrail, and more — perfect for cloud engineers, developers, and architects preparing for technical interviews.

---

## 📌 1. What all services are used in AWS?

AWS offers over **200 services** across various domains. Below are the most commonly used:

### ✅ Compute
- **EC2** – Virtual servers (Linux/Windows)
- **Lambda** – Serverless function execution
- **ECS / EKS** – Container orchestration

### ✅ Storage
- **S3** – Object storage
- **EBS** – Block storage for EC2
- **EFS** – Shared file storage

### ✅ Database
- **RDS** – Managed relational DBs (MySQL, PostgreSQL)
- **DynamoDB** – NoSQL, key-value store
- **Aurora**, **Redshift**, **DocumentDB**

### ✅ Networking
- **VPC** – Isolated network in the cloud
- **Route 53** – DNS and routing
- **API Gateway** – API hosting and management
- **CloudFront** – CDN for static content

### ✅ Security & Identity
- **IAM** – Access management
- **Cognito** – User sign-up and sign-in
- **KMS**, **Secrets Manager** – Encryption and secret storage

### ✅ Monitoring & Logging
- **CloudWatch** – Metrics, logs, dashboards
- **CloudTrail** – Auditing API calls

### ✅ Developer Tools
- **CodeBuild**, **CodeDeploy**, **CodePipeline** – CI/CD tools

### ✅ Machine Learning / AI
- **SageMaker** – ML model building
- **Rekognition** – Image/video recognition
- **Comprehend** – NLP services

### ✅ Use Case:
> In one project, we used:
> - **EC2** to host a Django API  
> - **S3** for storing user uploads  
> - **RDS (PostgreSQL)** for structured data  
> - **IAM** for managing dev/staging/prod access  
> - **CloudWatch** for log monitoring and alerting  
> - **CodePipeline** for CI/CD deployments

---

## 📌 2. How to configure S3 bucket access for one particular user?

### ✅ Goal:
Grant a specific IAM user access to a single bucket.

### ✅ IAM Policy Example:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": ["s3:ListBucket"],
      "Resource": "arn:aws:s3:::my-data-bucket"
    },
    {
      "Effect": "Allow",
      "Action": ["s3:GetObject", "s3:PutObject"],
      "Resource": "arn:aws:s3:::my-data-bucket/*"
    }
  ]
}
```

### ✅ Use Case:
> A user named `data-analyst` needed access to the `my-data-bucket` S3 bucket to upload and read reports.  
> I created an IAM policy scoped to only that bucket and attached it to the user to ensure least privilege access.

---

## 📌 3. How to provide S3 bucket access at folder level?

### ✅ Explanation:
S3 doesn’t have true folders; instead, it uses object key prefixes. You can restrict access to a specific "folder" by using IAM policies that target a prefix.

### ✅ IAM Policy Example:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": ["s3:GetObject", "s3:PutObject"],
      "Resource": "arn:aws:s3:::project-bucket/team-a/*"
    }
  ]
}
```

### ✅ Use Case:
> In a shared S3 bucket `project-bucket`, each team had its own folder such as `team-a/`, `team-b/`, etc.  
> I granted `team-a` access only to their own folder using a prefix-based policy.  
> This helped isolate access and avoid the need for multiple buckets.

---

## 📌 4. Any experience with CloudWatch Logs, CloudTrail, etc?

### ✅ CloudWatch Overview:
- Collects logs and metrics from AWS services like EC2, Lambda, ECS.
- Supports **alarms**, **dashboards**, and **Logs Insights** for querying logs.
- Useful for real-time monitoring and operational visibility.

### ✅ CloudTrail Overview:
- Records **API calls** across AWS services (via Console, CLI, SDK).
- Includes details such as the user, IP, resource, and action.
- Crucial for **auditing, compliance, and troubleshooting**.

### ✅ Use Case:
> A Lambda function was timing out, but no errors were visible in the app.  
> I used **CloudWatch Logs Insights** to discover that a call to an external API was failing intermittently.  
> Later, when an S3 bucket was mistakenly deleted, **CloudTrail** logs helped trace the delete action to a specific IAM user and script — enabling faster recovery and stronger access policies.

---

## 📌 5. VPC Related Questions

---

### 🔹 a. What is CIDR in a VPC?

**CIDR (Classless Inter-Domain Routing)** defines the IP address range of your VPC and subnets.

### ✅ Example:
- VPC: `10.0.0.0/16` → allows up to 65,536 IPs
- Public subnet: `10.0.1.0/24`
- Private subnet: `10.0.2.0/24`

### ✅ Use Case:
> I created a VPC with CIDR block `10.0.0.0/16` and divided it into `/24` subnets.  
> Public subnets hosted ALBs and Bastion hosts, while private subnets were used for EC2 and RDS.  
> This helped isolate workloads, control traffic, and implement fine-grained security.

---

### 🔹 b. What is a VPC Endpoint?

A **VPC Endpoint** allows secure, private connectivity from your VPC to AWS services without using an internet gateway, NAT, or public IPs.

### ✅ Types:
- **Gateway Endpoint** – Used for S3 and DynamoDB.
- **Interface Endpoint** – Used for services like SSM, Secrets Manager, KMS.

### ✅ Use Case:
> We had EC2 instances in private subnets that needed to access S3 for file storage.  
> Instead of using a NAT Gateway (which adds cost), I created a **Gateway VPC Endpoint**.  
> This reduced cost and improved security by keeping all traffic within AWS's private network.

---

### 🔹 c. What is VPC Peering?

**VPC Peering** allows private IP traffic between two VPCs.

### ✅ Key Characteristics:
- Low-latency and secure.
- Requires route table and security group updates.
- **No transitive routing** (A → B, B → C ≠ A → C).

### ✅ Use Case:
> We had a `data-lake` VPC and a `production` VPC in different AWS accounts.  
> I established VPC Peering to allow secure access between the two environments.  
> After updating routing tables and security groups, services could communicate using private IPs without exposing traffic to the public internet.

---

## ✅ Summary

This guide covered:

- ✅ AWS core services overview  
- ✅ S3 bucket and folder-level IAM access  
- ✅ Monitoring and auditing with CloudWatch and CloudTrail  
- ✅ VPC concepts: CIDR, Endpoints, and Peering  
- ✅ Real-world examples and production-ready use cases

---

_Use this guide as a cheat sheet or starter pack for AWS technical interviews._  
Feel free to extend it with Terraform examples or architecture diagrams. Good luck! 🚀
