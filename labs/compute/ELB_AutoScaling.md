# High Availability: ELB & Auto Scaling

**Project Duration:** ~60 Minutes
**Tech Stack:** AWS EC2, Auto Scaling Groups (ASG), Elastic Load Balancer (ELB), Apache Web Server

## Project Overview
In this lab, I moved beyond launching single instances to creating a highly available architecture. The goal was to ensure that my application could handle traffic spikes and hardware failures automatically. I configured an **Application Load Balancer** to distribute traffic and an **Auto Scaling Group** to dynamically add or remove servers based on CPU utilization.

## Key Objectives Achieved
* **High Availability:** Designed an architecture that survives the failure of a single Availability Zone.
* **Load Balancing:** Configured an ALB to route incoming HTTP traffic across multiple targets.
* **Dynamic Scaling:** Created scaling policies to scale out (add servers) when CPU hits 60% and scale in (remove servers) when traffic drops.
* **Stress Testing:** Simulating high load on the server to trigger automatic scaling actions.

## Step-by-Step Walkthrough

### 1. Creating the Launch Template
Instead of launching instances manually, I defined a "blueprint" (Launch Template) for my servers.
* **OS:** Amazon Linux 2023
* **User Data:** I wrote a bootstrap script to automatically install an Apache web server and display the instance's hostname upon launch.

### 2. Configuring the Load Balancer
I set up an **Application Load Balancer (ALB)** facing the internet.
* **Listeners:** Configured to listen on Port 80 (HTTP).
* **Target Group:** Created a group to register my instances. I set up a "Health Check" so the balancer would stop sending traffic to any server that failed to respond.

### 3. Setting up Auto Scaling
I created an **Auto Scaling Group (ASG)** using my template.
* **Capacity:** Set minimum to 1 instance and maximum to 3.
* **Scaling Policy:** I configured a "Target Tracking" policy to maintain average CPU utilization at 60%.

### 4. The Stress Test
To prove it worked, I connected to one instance and ran a stress tool to spike the CPU to 100%.
* **Result:** Within minutes, CloudWatch triggered an alarm.
* **Action:** The Auto Scaling Group automatically launched a second instance.
* **Verification:** I refreshed my browser and saw the Load Balancer flip-flopping traffic between the two healthy instances.

---

## Reflection
This lab was critical in understanding how "The Cloud" actually works at scale. It demonstrated that reliability isn't about making one perfect server that never crashes, but about building a system that treats servers as disposable resources that can be replaced automatically.
