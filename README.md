# AWS EC2 Webpage Deployment with ALB, ACM Certificate, and DNS Hosting

This project showcases the deployment of a webpage on an Apache web server hosted on an AWS EC2 instance. The setup integrates an Application Load Balancer (ALB), DNS hosting and registration using Route 53, and an ACM SSL certificate for secure connections.

---

## Features

- Hosted a webpage using an Apache Web Server on an EC2 instance.
- Configured an Application Load Balancer (ALB) for traffic distribution.
- Secured the setup with an AWS ACM SSL/TLS certificate.
- Linked a custom domain using DNS hosting and registration in Route 53.
- Ensured a scalable and secure deployment.

---

## Steps to Recreate

### 1. **Launch EC2 Instance**
   - Create an EC2 instance with the following configuration:
     - **AMI**: Amazon Linux 2
     - **Security Group**: Allow HTTP (port 80), HTTPS (port 443), and SSH (port 22).
     - **User Data**: Automate Apache Web Server installation and webpage setup:
       ```bash
       #!/bin/bash
       yum update -y
       yum install -y httpd
       systemctl start httpd
       systemctl enable httpd
       echo "<h1>Hello, Secure World!</h1>" > /var/www/html/index.html
       ```

### 2. **Create a Target Group**
   - Navigate to **EC2 > Target Groups** and create a new target group:
     - **Target Type**: Instances
     - **Protocol**: HTTP
     - Register your EC2 instance as a target.

### 3. **Set Up an Application Load Balancer (ALB)**
   - Under **EC2 > Load Balancers**, create an Application Load Balancer:
     - **Scheme**: Internet-facing
     - **Listeners**: Add HTTP (port 80) and HTTPS (port 443).
     - Attach the previously created target group to the ALB.

### 4. **Request an ACM SSL Certificate**
   - Go to **AWS Certificate Manager (ACM)** and request a certificate for your custom domain:
     - **Validation**: DNS validation.
     - Add the validation CNAME record to your Route 53 hosted zone.

### 5. **Configure a Hosted Zone in Route 53**
   - Register a domain or use an existing one in **Route 53**.
   - Create an A record to map your domain to the ALB's DNS name.

### 6. **Attach the ACM Certificate to ALB**
   - Add the ACM SSL certificate to the HTTPS listener of your ALB.

---

## How to Access the Webpage

1. Visit your custom domain via `https://<your-domain>` in any web browser.
2. The webpage is securely served through the Application Load Balancer with SSL encryption.

---

## Folder Structure

```plaintext
aws-ec2-alb-acm-dns-webpage/
├── Apache_Webpage_Files/       # Optional: Store any Apache-related configurations or HTML files
├── README.md                   # Project documentation
└── Screenshots/                # Add screenshots of AWS configuration and setup steps


---


Requirements
AWS Account (Free Tier or higher).
A registered domain name (Route 53 or another registrar).
Basic knowledge of AWS services (EC2, ALB, ACM, and Route 53).
---
Future Enhancements
Automate the entire setup using Terraform or AWS CloudFormation.
Add a CI/CD pipeline for continuous deployment.
Implement monitoring with AWS CloudWatch.
