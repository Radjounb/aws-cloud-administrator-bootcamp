# Troubleshooting Notes

## CloudWatch Agent Metrics Worked but Logs Did Not

### Initial state

The CloudWatch Agent reported `running` and `configured`. Custom metrics appeared in the `CWAgent` namespace, but no Apache log groups appeared.

### Checks performed

1. Confirmed Apache was running.
2. Confirmed local access and error log files existed.
3. Confirmed HTTP requests were being written to the access log.
4. Validated the CloudWatch Agent JSON configuration.
5. Confirmed the EC2 IAM role had Systems Manager and CloudWatch Agent permissions.
6. Tested reading the log files as the `cwagent` service account.
7. Inspected permissions on every component of the filesystem path.

### Root cause

The CloudWatch Agent service account could not traverse the Apache log directory, even though the log files themselves appeared readable under their group permissions.

### Fix

Targeted ACL permissions were applied to allow `cwagent` to traverse the Apache log directory and read the access/error log files. The agent was restarted and log ingestion was verified in CloudWatch.

### Lesson

A healthy agent and correct IAM policy do not guarantee log delivery. Linux filesystem traversal permissions must also permit the service account to reach and read the source files.
