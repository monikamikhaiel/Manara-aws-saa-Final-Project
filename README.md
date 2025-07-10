# Manara-aws-saa-Final-Project

Project Documentation: Scalable Web Application with ALB and Auto Scaling

1. Introduction
This documentation outlines the architecture and deployment of a highly scalable, fault-tolerant web application utilizing Amazon Web Services (AWS) infrastructure. The core components of this architecture are the Application Load Balancer (ALB) and Auto Scaling Group (ASG), designed to ensure high availability and dynamic capacity management.

1.1. Project Overview
The goal of this project is to create a robust web application environment capable of handling varying traffic loads without manual intervention. By distributing incoming traffic across multiple instances using an ALB and automatically adjusting the number of instances based on demand using an ASG, the application maintains performance and minimizes downtime.

1.2. Key AWS Services Utilized
 - Amazon EC2 (Elastic Compute Cloud): Hosts the web application instances.

 - Amazon VPC (Virtual Private Cloud): Provides a secure and isolated network environment.

 - Application Load Balancer (ALB): Distributes incoming HTTP/HTTPS traffic to targets (EC2 instances).

 - Auto Scaling Group (ASG): Manages the fleet of EC2 instances, ensuring optimal capacity.

 - EC2 Launch Template: Defines the configuration for instances launched by the ASG.

 - Amazon CloudWatch: Monitors metrics and triggers scaling events.

2. Architecture Overview
The architecture is designed for high availability across multiple Availability Zones (AZs) within a VPC.

2.1. Architectural Diagram (Conceptual)
Users access the application via a DNS endpoint (e.g., provided by Route 53).

ALB (Application Load Balancer): Receives the traffic and distributes it across the available instances in the ASG. The ALB is typically placed in public subnets across multiple AZs.

Auto Scaling Group (ASG):

Manages EC2 instances hosting the web application.

The instances are placed in private subnets for enhanced security.

The ASG ensures that a minimum number of instances are always running and scales out (adds instances) or scales in (removes instances) based on `load`.

Target Group: The ALB routes traffic to a Target Group, which consists of the EC2 instances managed by the ASG.

Amazon CloudWatch Alarms: Monitor metrics (e.g., CPU utilization, request count) to trigger ASG scaling policies.

3. Implementation Steps
The following steps detail the deployment process for the scalable web application.

3.1. Network Infrastructure

  - Create VPC with at least two public and two private subnets across two distinct Availability Zones.
  - Create 2 route tables one for public and one for private route.
  - Create an Internet Gateway (IGW) along with  NAT Gateway (located in a public subnet) for outbound internet access.
  - Create Application Load Balancer (ALB)
  - Create 2 Security Groups one for ALB access and the other one for EC2 
      * ALB Security Group:
      
      Inbound: Allow HTTP (Port 80) and HTTPS (Port 443) traffic from 0.0.0.0/0 (internet).
      
      Outbound: Allow traffic to the EC2 instances (Web Server Security Group).
      
      * Web Server Security Group:
      
      Inbound: Allow HTTP (Port 80) and HTTPS (Port 443) traffic only from the ALB Security Group.
      
      Outbound: Allow necessary outbound traffic (e.g., to databases, external APIs).

3.2. Compute Infrastructure
  - Create The Launch Template that defines the specifications for instances managed by the ASG along with User Data including a script to automate application installation and configuration .
  - Create ASG (Desired: 2 ; Minimum: 1 ; Maximum: 4 ) with Target Tracking Scaling Policy
      Metric: such as CPUUtilization (for compute-bound apps) or RequestCountPerTarget (from ALB, for load-bound apps).
  - Create target group pointing to ASG
  - Create listener with (HTTPS 443)
  - Associate the listener with the Target Group created above
    
