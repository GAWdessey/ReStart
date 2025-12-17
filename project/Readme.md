# Cloud Engineering Projects Overview

This directory contains the documentation and architectural details for the projects I have designed and deployed during my AWS cloud engineering journey. Each project was selected to target specific domains of cloud infrastructure, from serverless automation to global scalability and conversational AI.

Below is a summary of the technical scope and the core competencies demonstrated in each solution.

---
## Cumulative Skills Matrix

| Skill Category | Technologies Applied |
| :--- | :--- |
| **Compute & Serverless** | AWS Lambda, EC2, Auto Scaling Groups |
| **Storage & CDN** | Amazon S3, Amazon EFS, Amazon CloudFront |
| **Databases** | Amazon DynamoDB (NoSQL), Amazon RDS (Relational) |
| **Security & Identity** | IAM Roles & Policies, AWS WAF, Security Groups |
| **AI & Machine Learning** | Amazon Lex V2, Conversation Design |
| **DevOps & Monitoring** | CloudWatch, Trusted Advisor |
---

## Project 1: Serverless Restaurant Operations System
**Focus:** Serverless Architecture & Database Integration

### Project Scope
I modernized a local restaurant's operational workflow by migrating them from manual pen-and-paper tracking to a fully automated, cloud-native system. The goal was to eliminate server management overhead while ensuring real-time data accuracy for inventory and orders.

### My Technical Contributions
* **Architected a Serverless Backend:** I utilized **AWS Lambda** to process booking requests and order data without provisioning or managing a single server.
* **Designed NoSQL Data Models:** I implemented **Amazon DynamoDB** to handle high-velocity data ingestion, designing efficient tables for order history and inventory tracking.
* **Deployed Static Web Hosting:** I hosted the client-facing front end on **Amazon S3**, configuring bucket policies for public access while maintaining security.

**Key Skills Used:** AWS Lambda, Amazon DynamoDB, Amazon S3, Event-Driven Architecture.

---

## Project 2: Global 3D E-Commerce Infrastructure
**Focus:** High Availability, Scalability & Content Delivery

### Project Scope
I designed a robust infrastructure capable of supporting a high-traffic 3D asset marketplace. The primary challenge was handling heavy media files (3D models) with low latency for a global user base while ensuring the application could survive sudden traffic spikes.

### My Technical Contributions
* **Implemented Auto Scaling:** I configured **Amazon EC2 Auto Scaling** groups behind an Application Load Balancer to dynamically adjust compute capacity based on CPU utilization.
* **Optimized Content Delivery:** I deployed **Amazon CloudFront** as a CDN (Content Delivery Network) to cache heavy 3D assets at edge locations, significantly reducing load times for international users.
* **Secured the Perimeter:** I integrated **AWS WAF (Web Application Firewall)** to protect against common web exploits and used **Trusted Advisor** to audit the environment for cost and security gaps.
* **Managed Hybrid Database Storage:** I utilized **Amazon RDS** for transactional order data and DynamoDB for product catalogs.

**Key Skills Used:** EC2 Auto Scaling, Elastic Load Balancing (ELB), Amazon CloudFront, AWS WAF, Amazon RDS.

---

## Project 3: AI-Powered Assessment Bot ('S3Quiz')
**Focus:** Artificial Intelligence (NLP), Logic & IAM Security

### Project Scope
I engineered an interactive chatbot to automate technical assessments. Unlike the previous infrastructure projects, this focused on logic, user interaction, and securing development environments for team collaboration.

### My Technical Contributions
* **Engineered Conversation Flows:** I built the bot using **Amazon Lex V2**, designing intents and slots to capture user data naturally.
* **Programmed Conditional Logic:** I wrote complex branching logic to validate user answers in real-time (`{Slot} = "Value"`), enabling the bot to provide immediate, context-aware feedback.
* **Enforced Least Privilege Security:** I created custom **IAM Policies** and Users. I demonstrated granular access control by allowing a test user to invoke the bot without having write-access to the underlying architecture.

**Key Skills Used:** Amazon Lex V2, Natural Language Processing (NLP), AWS IAM (Identity & Access Management), Python/Logic flows.

---
