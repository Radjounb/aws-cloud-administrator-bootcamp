# AWS Cloud Administrator Bootcamp

A hands-on AWS administration portfolio project focused on building, operating, monitoring, securing, and troubleshooting a multi-service AWS environment.

## Project Overview

This project documents practical AWS Cloud Administrator work completed through an end-to-end lab. The environment was built to practice core cloud administration skills rather than simply follow console walkthroughs.

## AWS Services & Skills

- **Networking:** Amazon VPC, public/private subnets, route tables, Internet Gateway, NAT, multi-AZ design
- **Compute:** Amazon EC2, Amazon Linux, Apache HTTP Server, user data, instance administration
- **Identity & Access:** IAM roles, policies, least privilege, instance profiles, MFA concepts
- **Systems Management:** AWS Systems Manager Session Manager for administrative access
- **Storage:** Amazon EBS, S3, EFS, filesystem mounting and persistence
- **Database:** Amazon RDS for MySQL, private database networking, backups/snapshots
- **Availability & Scaling:** Application Load Balancer, target groups, launch templates, Auto Scaling
- **Monitoring:** Amazon CloudWatch metrics, alarms, CloudWatch Agent, custom OS metrics
- **Logging:** Centralized Apache access/error logs in CloudWatch Logs
- **Notifications:** Amazon SNS alerting
- **Security:** Security groups, private-resource design, IAM permissions, controlled administrative access
- **Operations:** Linux troubleshooting, service validation, disk management, cost awareness and cleanup

## Monitoring Validation

The environment includes a CloudWatch alarm for EC2 CPU utilization. A controlled CPU-load test was used to verify the complete monitoring path:

```text
EC2 CPU Load
     ↓
CloudWatch CPUUtilization
     ↓
CloudWatch Alarm
     ↓
Amazon SNS
     ↓
Notification
```

After the load was stopped, CPU utilization returned to normal and the alarm recovered to the OK state.

The CloudWatch Agent was also configured to publish operating-system metrics such as memory and disk utilization.

## Centralized Apache Logging

Apache logs from the EC2 web server are centralized in CloudWatch Logs:

```text
/var/log/httpd/access_log
        ↓
Amazon CloudWatch Agent
        ↓
/enterprise/web01/apache/access

/var/log/httpd/error_log
        ↓
Amazon CloudWatch Agent
        ↓
/enterprise/web01/apache/error
```

Log retention was configured for 7 days for the lab environment.

## Troubleshooting Case Study

One of the most useful exercises involved troubleshooting CloudWatch log ingestion.

### Symptoms

- CloudWatch Agent was running.
- Agent configuration validated successfully.
- Custom memory metrics were reaching CloudWatch.
- Apache was producing logs locally.
- CloudWatch log groups were not receiving the Apache logs.

### Investigation

The IAM role and CloudWatch Agent configuration were verified first. Testing the agent service account directly revealed that `cwagent` could not read the Apache logs.

The underlying Apache log directory permissions prevented the CloudWatch Agent service account from traversing the directory.

### Resolution

Narrow ACL permissions were granted so the CloudWatch Agent could traverse the Apache log directory and read the required log files. After restarting the agent, both Apache log groups appeared in CloudWatch and real HTTP request events were successfully ingested.

This demonstrated a practical troubleshooting workflow:

```text
Verify service
→ Verify configuration
→ Verify IAM
→ Verify source logs
→ Test service-account permissions
→ Identify filesystem restriction
→ Apply targeted fix
→ Validate end-to-end
```

## Operational Verification

Final validation included:

- Apache service running successfully
- Web application responding locally over HTTP
- CloudWatch Agent running and configured
- EBS filesystems mounted and healthy
- EC2 instance successfully assuming its IAM role
- CloudWatch custom metrics available
- Apache access/error events visible in CloudWatch Logs

## Security Note

This repository intentionally excludes AWS credentials, private keys, database passwords, account identifiers, and other secrets. Configuration examples are sanitized before publication.

## Repository Structure

```text
.
├── README.md
├── cloudwatch/
│   └── cloudwatch-agent-config.json
├── documentation/
│   ├── architecture.md
│   ├── monitoring.md
│   └── troubleshooting.md
└── scripts/
    └── README.md
```

## Portfolio Outcome

The project demonstrates hands-on experience with AWS cloud administration, Linux systems administration, IAM, networking, monitoring, centralized logging, high-availability concepts, troubleshooting, and cost-conscious resource management.
