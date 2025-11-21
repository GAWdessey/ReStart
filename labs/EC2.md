# Setting up and launching an EC2 instance

## Topics covered

**1. Launch a web server with termination protection enabled**

**2. Monitor Your EC2 instance**

**3. Modify the security group that your web server is using to allow HTTP access**

**4. Resize your Amazon EC2 instance to scale**

**5. Test termination protection**

**6. Terminate your EC2 instance**

![EC2](../images/ec2.PNG)

## Launch a web server with termination protection enabled

In this task, you will launch an Amazon EC2 instance with termination protection. Termination protection prevents you from accidentally terminating an EC2 instance. You will deploy your instance with a User Data script that will allow you to deploy a simple web server.
In the AWS Management Console on the Services menu, choose EC2.
In the left navigation pane, choose EC2 Dashboard to ensure that you are on the dashboard page.
Choose Launch instance, and then select Launch instance.
![EC2](../images/AWSDashboard.PNG)

## Choosing an Amazon Machine Image (AMI)
An AMI provides the information required to launch an instance, which is a virtual server in the cloud. An AMI includes the following:
A template for the root volume for the instance (for example, an operating system or an application server with applications)
Launch permissions that control which AWS accounts can use the AMI to launch instances
A block device mapping that specifies the volumes to attach to the instance when it is launched
The Quick Start list contains the most commonly used AMIs. You can also create your own AMI or select an AMI from the AWS Marketplace, an online store where you can sell or buy software that runs on AWS.
Locate the Application and OS Images (Amazon Machine Image) pane. 
Under AMI Machine Image (AMI), notice that the Amazon Linux 2023* image is selected by default. Keep this setting.
![EC2](../images/ChoosinganAMI.PNG)

## Choosing an instance type
Amazon EC2 provides a wide selection of instance types optimized to fit different use cases. 
Instance types comprise varying combinations of CPU, memory, storage, and networking capacity and give you the flexibility to choose the appropriate mix of resources for your applications. 
Each instance type includes one or more instance sizes so that you can scale your resources to the requirements of your target workload.
![EC2](../images/ChoosinganInstancetype.PNG)

## Configuring the network settings
The VPC indicates which virtual private cloud (VPC) you want to launch the instance into. You can have multiple VPCs, including different ones for development, testing, and production.
In the Network settings pane, choose Edit
For VPC - required, select Lab VPC.
Still in the Network settings pane, configure the Security Group as follows:
Security group name - required: Web Server security group
Description: Security group for my web server
A security group acts as a virtual firewall that controls the traffic for one or more instances. When you launch an instance, you associate one or more security groups with the instance. You add rules to each security group that allow traffic to or from its associated instances. You can modify the rules for a security group at any time; the new rules are automatically applied to all instances that are associated with the security group.
Under Inbound security groups rules select the Remove
In this lab, you will not log into your instance using SSH. Removing SSH access will improve the security of the instance.
![EC2](../images/NetworkConfiguration.PNG)

## Adding storage
Amazon EC2 stores data on a network-attached virtual disk called Amazon Elastic Block Store (Amazon EBS).
You launch the EC2 instance using a default 8 GiB disk volume. This is your root volume (also known as a boot volume).
![EC2](../images/Configurestorageneeded.PNG)

## Configuring advanced details 
Expand the Advanced details pane.
Select the dropdown for Termination protection, then choose Enable.
When you launch an instance in Amazon EC2, you have the option of passing user data to the instance. These commands can be used to perform common automated configuration tasks and even run scripts after the instance starts. 
Copy the following commands, and paste them into the User data text box.

#!/bin/bash
yum -y install httpd
systemctl enable httpd
systemctl start httpd
echo '<html><h1>Hello From Your Web Server!</h1></html>' > /var/www/html/index.html

The script does the following:
Install an Apache web server (httpd)
Configure the web server to automatically start on boot
Activate the Web server
Create a simple web page
![EC2](../images/ConfiguringUserData.PNG)

## Launching an EC2 instance
Now that you have configured your EC2 instance settings, it is time to launch your instance.
In the right pane, choose Launch instance
Choose View all instances
The instance appears in a Pending state, which means it is being launched. It then changes to Running, which indicates that the instance has started booting. There will be a short time before you can access the instance.
The instance receives a public DNS name that you can use to contact the instance from the Internet.
Select the  box next to your Web Server. The Details tab displays detailed information about your instance.
To view more information in the Details tab, drag the window divider upward.
Review the information displayed in the Details, Security and Networking tabs.
Wait for your instance to display the following:
Note: Refresh if needed.
Instance State:  Running
Status Checks:   2/2 checks passed
![EC2](../images/RunningEC2.PNG)

## Monitor Your Instance
Monitoring is an important part of maintaining the reliability, availability, and performance of your Amazon Elastic Compute Cloud (Amazon EC2) instances and your AWS solutions.
Select the instance by checking the box next to the instance and navigate to the bottom of the screen to the Status checks tab.
With instance status monitoring, you can quickly determine whether Amazon EC2 has detected any problems that might prevent your instances from running applications. Amazon EC2 performs automated checks on every running EC2 instance to identify hardware and software issues.
Notice that both the System reachability and Instance reachability checks have passed.
Select the Monitoring tab.
This tab displays Amazon CloudWatch metrics for your instance. Currently, there are not many metrics to display because the instance was recently launched.
You can choose a graph to see an expanded view.
Amazon EC2 sends metrics to Amazon CloudWatch for your EC2 instances. Basic (five-minute) monitoring is enabled by default. You can enable detailed (one-minute) monitoring.
In the Actions  menu, select Monitor and troubleshoot  Get Instance Screenshot.
This shows you what your Amazon EC2 instance console would look like if a screen were attached to it.
![EC2](../images/EC2screenshot.PNG)

## Update Your Security Group and Access the Web Server
When you launched the EC2 instance, you provided a script that installed a web server and created a simple web page. In this task, you will access content from the web server.
Select the instance by checking the box and select the Details tab.
Copy the Public IPv4 address of your instance to your clipboard.
Open a new tab in your web browser, paste the IP address you just copied, then press Enter.
Question: Are you able to access your web server? Why not?
You are not currently able to access your web server because the security group is not permitting inbound traffic on port 80, which is used for HTTP web requests. This is a demonstration of using a security group as a firewall to restrict the network traffic that is allowed in and out of an instance.
To correct this, you will now update the security group to permit web traffic on port 80.
Keep the browser tab open, but return to the EC2 Management Console tab.
In the left navigation pane, select Security Groups located under Network & Security.
Select  Web Server security group.
Select the Inbound rules tab.
The security group currently has no rules.

Select Edit inbound rules then select Add rule and configure the rule with the following settings:
Type: HTTP
Source: Anywhere-IPv4
Select Save rules
Return to the web server tab that you previously opened and refresh  the page.
You should see the message Hello From Your Web Server!
