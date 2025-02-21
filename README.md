# Deploying a Three-Tier AWS Architecture for a Static Website

## Project Background:

I recently wrapped up a DevOps project where the goal was to host a static HTML website on AWS. The focus was on building a robust infrastructure for the website, making it highly available, fault-tolerant, and scalable.

![Final Architecture](https://github.com/user-attachments/assets/932400b2-83bb-4ed9-88a3-7b283d261f1a)


## Architecture Overview:

### Three-Tier VPC Setup:

1. **Tiers Setup:**
   - Created a three-tier AWS VPC with public and private subnets in 2 availability zones.

2. **Subnet Organization:**
   - Tier 1 housed the public subnet with Nat Gateway, Load Balancer, and Bastion Host.
   - Tier 2 had the private subnet for webservers.
   - Tier 3 featured another private subnet for the database.

### Infrastructure Steps:

1. **VPC Creation:**
   - Started by setting up a VPC named "Dev VPC."

2. **DNS Hostnames:**
   - Enabled DNS Hostnames in the VPC.

3. **Internet Gateway:**
   - Set up an Internet Gateway for communication with the internet.

4. **Subnets and Route Tables:**
   - Established public and private subnets in two Availability Zones.
   - Created route tables, associating public subnets with the public route table.

![VPC Architecture 1](https://github.com/user-attachments/assets/c477bcd5-4090-4f25-9339-d20576af1e94)


5. **NAT Gateway:**
   - Deployed NAT Gateways in both Availability Zones for internet access in private subnets.
  
![Nat_Gateway_Reference_Architecture copy](https://github.com/user-attachments/assets/b747679e-3473-4cf1-b2da-ee512635ddb4)


6. **Security Groups:**
   - Configured security groups for ALB, SSH, and webservers to control traffic.
  
![Security Group](https://github.com/user-attachments/assets/853a654a-565b-4930-9ab1-51ce50fbfa51)


7. **Key Pair:**
   - Generated a key pair for SSH access to instances.

8. **Webserver Deployment Script:**
   - Script to install the website on EC2 instances in private subnets.
     ```bash
     #!/bin/bash
     sudo su
     yum update -y
     yum install -y httpd
     cd /var/www/html
     wget https://github.com/azeezsalu/jupiter/archive/refs/heads/main.zip
     unzip main.zip
     cp -r jupiter-main/* /var/www/html/
     rm -rf jupiter-main main.zip
     systemctl enable httpd 
     systemctl start httpd
     ```
9. **Application Load Balancer (ALB):**
   - Launched EC2 instances in private app subnets and associated them with an ALB.
   - Configured a target group and associated it with the ALB.

10. **Route 53:**
    - Registered a new domain and created a record set to point the domain to the ALB.

11. **SSL Certificate:**
    - Obtained an SSL certificate from AWS Certificate Manager for secure communication.

12. **Auto Scaling Group (ASG):**
    - Created an ASG with a launch template, specifying EC2 configurations.
    - Configured desired, minimum, and maximum capacities.
    - Set up notifications and selected an SNS topic.

## Project Conclusion:

This project achieved the goal of hosting a static website on AWS with a three-tier architecture. The infrastructure is designed to ensure resilience, scalability, and effective management of web traffic.

<img width="1354" alt="Screenshot 2023-12-14 at 2 52 49 PM" src="https://github.com/user-attachments/assets/0c2c87b5-f14f-4809-a6b9-b589a0118510" />

   
