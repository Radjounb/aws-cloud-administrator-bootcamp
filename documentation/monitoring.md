# Monitoring and Logging

## EC2 Alarm

A CloudWatch alarm was configured for the web server's CPU utilization with a 70% threshold during the lab.

A controlled CPU load was generated on the instance to validate the alarm. The alarm entered ALARM state, the artificial load was terminated, and the alarm subsequently returned to OK.

## CloudWatch Agent

The CloudWatch Agent was installed on Amazon Linux and configured for 60-second host-level metric collection.

Metrics included:

- Memory utilization
- Disk utilization
- Disk inode availability
- Disk I/O time
- CPU operating-system metrics
- Swap utilization

The `CWAgent` namespace was verified in the CloudWatch console and `mem_used_percent` was confirmed.

## Apache Logs

The agent collects:

- `/var/log/httpd/access_log`
- `/var/log/httpd/error_log`

CloudWatch destinations:

- `/enterprise/web01/apache/access`
- `/enterprise/web01/apache/error`

Both were configured with 7-day retention for the lab.
