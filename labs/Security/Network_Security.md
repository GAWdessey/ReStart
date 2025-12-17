# Network Security: Security Groups & NACLs

**Project Focus:** Network Hardening and Traffic Filtering
**Tech Stack:** VPC, Security Groups, Network ACLs (NACL), EC2

## Project Overview
In this lab, I focused on securing the network layer of my AWS infrastructure. I explored the difference between **Security Groups** (stateful firewalls at the instance level) and **Network ACLs** (stateless firewalls at the subnet level). The objective was to implement a "Defense in Depth" strategy, ensuring strictly authorized traffic could reach my servers.

## Key Objectives Achieved
* **Stateful Filtering:** configured Security Groups to allow SSH (22) and HTTP (80) only from specific IP ranges.
* **Stateless Filtering:** Configured Network ACLs to block traffic at the subnet boundary.
* **Troubleshooting:** Diagnosed connectivity issues caused by mismatched inbound/outbound rules.
* **CIDR Notation:** Practiced defining precise IP ranges (e.g., `0.0.0.0/0` vs `192.168.1.0/24`).

## Step-by-Step Walkthrough

### 1. Security Group Configuration
I launched a web server and initially blocked all access.
* **Action:** I edited the Inbound Rules to allow Port 80 (HTTP) from anywhere.
* **Observation:** Because Security Groups are *stateful*, I did not need to configure an Outbound rule; the response traffic was allowed automatically.

### 2. Testing Network ACLs (NACLs)
To test subnet-level security, I created a custom NACL and associated it with my public subnet.
* **Scenario:** I explicitly denied traffic from a specific IP address while allowing everyone else.
* **Lesson:** Unlike Security Groups, NACLs are *stateless*. I learned that I must explicitly allow ephemeral ports (1024-65535) in the outbound rules, otherwise, the return traffic gets blocked even if the inbound request was allowed.

### 3. The "Defense in Depth" Test
I attempted to SSH into my instance.
* **Layer 1 (NACL):** Allowed my traffic into the subnet.
* **Layer 2 (Security Group):** I removed the SSH rule.
* **Result:** Connection timed out. This confirmed that even if a hacker gets past the network border (NACL), they are still stopped at the instance level (Security Group).

---

## Reflection
This lab clarified a major exam topic: the difference between stateful and stateless firewalls. I now understand that Security Groups are the primary defense for instances, while NACLs act as a broader guardrail for the entire subnet.
