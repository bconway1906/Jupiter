# Deploying a Three-Tier AWS Architecture for a Static Website

## Project Background:

I recently wrapped up a DevOps project where the goal was to host a static HTML website on AWS. The focus was on building a robust infrastructure for the website, making it highly available, fault-tolerant, and scalable.

![Final Architecture](https://github.com/bconway1906/Static-Website-AWS/assets/148906255/cb325ba8-3bf4-43ff-9d62-1d3c96e50dc0)

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
  
![VPC Architecture 1](https://github.com/bconway1906/Static-Website-AWS/assets/148906255/eb6c51b1-d82e-47af-9818-7e7a430ecd00)

5. **NAT Gateway:**
   - Deployed NAT Gateways in both Availability Zones for internet access in private subnets.
  
![Nat_Gateway_Reference_Architecture copy](https://github.com/bconway1906/Static-Website-AWS/assets/148906255/9201ccfc-a8bd-4701-bf9a-ab105076136d)

6. **Security Groups:**
   - Configured security groups for ALB, SSH, and webservers to control traffic.
  
![Security Group](https://github.com/bconway1906/Static-Website-AWS/assets/148906255/898a8a0d-0da8-43e0-b310-21696e527a35)

7. **Key Pair:**
   - Generated a key pair for SSH access to instances.

8. **Webserver Deployment Script:**
   - Developed a script to install the website on EC2 instances in private subnets.
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
     - Explanation:
        - `sudo su`: Switch to the superuser for executing commands with elevated privileges.
        - `yum update -y`: Update system packages to the latest versions.
        - `yum install -y httpd`: Install the Apache web server.
        - `cd /var/www/html`: Change to the web server directory.
        - `wget ...`: Download the website files from GitHub.
        - `unzip ...`: Unzip the downloaded files.
        - `cp ...`: Copy the unzipped files to the web server directory.
        - `rm -rf ...`: Remove unnecessary files.
        - `systemctl enable httpd`: Enable the Apache service to start on boot.
        - `systemctl start httpd`: Start the Apache service.

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

<img width="1354" alt="Screenshot 2023-12-14 at 2 52 49 PM" src="https://github.com/bconway1906/Static-Website-AWS/assets/148906255/d89b6dcd-e116-457b-bfb7-5d0d6e2c36a8">
   
