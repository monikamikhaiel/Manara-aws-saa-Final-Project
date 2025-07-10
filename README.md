# Manara-aws-saa-Final-Project

## Infrastructure Setup

  1. Network Configuration
  VPC with public subnets in at least 2 availability zones.
  
  Internet Gateway for external access.
  
  Route Tables configured for public internet access.
  
  2. Security Groups
  ALB SG: Allows inbound HTTP (80) and/or HTTPS (443).
  
  EC2 SG: Allows traffic only from ALB security group on application port (e.g., 80).
  
  3. Launch Template
  Amazon Linux 2 AMI containing the user data allowing to install dependencies along with the web app.
  
  4. Auto Scaling Group
  Configured with:
  
  Minimum capacity: 2
  
  Desired capacity: 2
  
  Maximum capacity: 5
  
  Attached to multiple subnets (multi-AZ)
  
  Uses target tracking or step scaling policies based on CloudWatch metrics.
  
  5. Application Load Balancer @ public subnet
  Internet-facing
  
  Listener on port 80/443
  
  Target group health checks (e.g., /health endpoint)
  
  Routes traffic to healthy instances in the ASG.
  6. Bastion Host @ public subnet 
  EC2 in public subnet to allow to access the worker EC2 
