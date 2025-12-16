# 3D E-Commerce Architecture On AWS

**Group 3** | **Date:** 01-12-2025

![Architecture Diagram](path/to/your/architecture_diagram_image.png)
*Architecture diagram*

---

## 1. Introduction
This document explains the AWS architecture designed to support a next-generation 3D e-commerce web application. The platform must be globally available, highly scalable, secure, fast, and cost-efficient while rendering 3D models to millions of users.

Below is a breakdown of the AWS services selected, why they were chosen, and how the final solution meets all business and technical requirements.

## 2. AWS Services

### 1. Amazon S3 – Storage for 3D Assets
Amazon S3 is used to store all 3D product models, textures, images, and static website assets.

**Why chosen:**
* Highly durable (11 9’s durability)
* Scales automatically for millions of asset requests
* Integrates directly with CloudFront for low-latency delivery
* Cost-effective object storage

### 2. Amazon CloudFront – Global CDN for Content Delivery
CloudFront caches 3D assets, images, and website static files across global edge locations.

**Why chosen:**
* Reduces latency for global users
* Protects the backend using AWS Shield and WAF integration
* Improves performance for heavy 3D content
* Decreases S3 and backend load

### 3. EC2 Auto Scaling / AWS Lambda – Backend Compute
The backend API can be built using either:
* **EC2 Auto Scaling** for applications requiring long-running processes
* **AWS Lambda** for event-driven API logic (serverless)

**Why chosen:**
* Supports unpredictable spikes in traffic
* Load adjusts automatically up or down
* Reduces cost by only paying for what is used
* Lambda improves scalability and removes server maintenance

### 4. Amazon RDS / DynamoDB – User & Product Database
The architecture uses:
* **RDS (MySQL/PostgreSQL)** for structured data like orders, users, and transactions
* **DynamoDB** for fast product lookups and real-time inventory access

**Why chosen:**
* RDS provides strong consistency & relational queries
* DynamoDB offers millisecond latency for product browsing
* Both scale easily and integrate with Lambda/EC2

### 5. Elastic Load Balancer (ELB) – Traffic Distribution
The ALB (Application Load Balancer) distributes user requests across EC2 instances or Lambda functions.

**Why chosen:**
* Provides health checks and fault tolerance
* Supports path-based routing (important for API and asset endpoints)
* Ensures high availability and automatic failover

### 6. Amazon Route 53 – Domain & DNS Management
Route 53 manages the domain and routes traffic to CloudFront or ELB.

**Why chosen:**
* Highly available DNS service
* Supports health checks and routing policies
* Integrates well with CloudFront for global performance

### 7. CloudWatch & Trusted Advisor – Monitoring & Optimization
These services monitor logs, application performance, and system metrics.

**Why chosen:**
* **CloudWatch** provides real-time alerts, metrics, and dashboards
* **Trusted Advisor** helps reduce cost, improve security, and detect misconfigurations
* Ensures the platform remains reliable and optimized

---

## 3. How The Architecture Meets Requirements

### 1. High Availability
* Route 53 + CloudFront provide multi-region distribution
* ELB supports auto failover
* EC2 Auto Scaling ensures instances are replaced automatically
* S3 provides 99.99% availability for assets

### 2. Scalability
* Auto Scaling handles unpredictable traffic spikes
* DynamoDB offers on-demand scaling
* CloudFront caches content globally, reducing backend load
* Lambda automatically scales to thousands of requests per second

### 3. Performance
* CloudFront drastically reduces latency for 3D models
* DynamoDB provides single-digit millisecond reads
* EC2 instances or Lambda functions process API requests quickly
* S3 ensures fast retrieval for large 3D assets

### 4. Security
* AWS WAF on CloudFront blocks malicious traffic
* IAM roles restrict resource access
* Security groups protect EC2 instances
* RDS has encryption, backups, and Multi-AZ deployment
* S3 supports bucket policies and encryption

### 5. Cost Optimization
* S3 and CloudFront reduce expensive compute usage
* Auto Scaling prevents over-provisioning
* Lambda eliminates server idle cost
* Trusted Advisor recommends savings opportunities
* DynamoDB on-demand mode reduces unnecessary database cost

---

## 4. Design Trade-Offs or Challenges

**Challenge 1: Handling large 3D model files**
* **Solution:** Compress assets, use CloudFront caching, and enable S3 Transfer Acceleration.

**Challenge 2: Balancing cost and performance**
* **Solution:** Use Lambda for low-traffic periods and EC2 Auto Scaling for peak hours.

**Challenge 3: Managing two databases (RDS + DynamoDB)**
* **Solution:** Use RDS for transactional consistency (orders) and DynamoDB for high-speed reads (products).
* This hybrid approach ensures both performance and structure.

---

## Conclusion
This AWS architecture fully supports a global 3D e-commerce platform by delivering high availability, speed, scalability, security, and cost-efficiency. The selected AWS services work together to create a modern, reliable infrastructure capable of serving millions of users.
