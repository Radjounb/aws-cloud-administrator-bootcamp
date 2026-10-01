# Architecture

## Design

The bootcamp environment used a custom Amazon VPC with public and private subnet design across multiple Availability Zones.

Core design goals:

- Separate internet-facing and private resources.
- Keep database workloads private.
- Use security groups to restrict traffic between tiers.
- Use IAM roles instead of embedding AWS credentials on EC2.
- Use Systems Manager Session Manager for administrative access.
- Centralize monitoring and logs in CloudWatch.
- Practice load balancing and Auto Scaling concepts for availability.

## Logical Architecture

```text
Internet
   |
Internet Gateway
   |
Public Subnets
   |
Application Load Balancer
   |
EC2 Web Tier
   |
Security Group Controls
   |
Private Database Tier
   |
Amazon RDS MySQL

EC2
 ├── IAM Instance Role
 ├── Systems Manager
 ├── EBS
 ├── CloudWatch Agent
 └── Apache

CloudWatch
 ├── EC2 Metrics
 ├── CWAgent OS Metrics
 ├── Alarms
 └── Apache Logs
        |
       SNS
```

## Availability

Multiple subnets/AZs, load balancing, launch templates, and Auto Scaling were practiced to understand how AWS workloads can avoid dependence on a single server or Availability Zone.

> This repository documents a training environment. Resource counts and sizes were intentionally kept small to control lab costs.
