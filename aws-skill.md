---
name: aws-readonly-check
version: 1.0
description: Perform a small set of safe, read-only AWS checks for basic infrastructure visibility. Never modify resources, retrieve secrets, or change permissions.
---

# AWS Read-Only Check

## Objective

Perform basic, read-only AWS checks and report the results clearly.

## Allowed actions

Run only read-only AWS commands that inspect:

1. Current AWS identity.
2. Current AWS region.
3. EC2 instance count and basic status information.
4. CloudWatch log group names only.

## Suggested commands

Use the configured AWS CLI profile and region.

### 1. Check AWS identity

```bash
aws sts get-caller-identity
```

### 2. Check EC2 instances

```bash
aws ec2 describe-instances \
  --query 'Reservations[].Instances[].{InstanceId:InstanceId,State:State.Name,Type:InstanceType}' \
  --output table
```

### 3. List CloudWatch log groups

```bash
aws logs describe-log-groups \
  --query 'logGroups[].logGroupName' \
  --output table
```

## Security restrictions

- Do not create, modify, or delete AWS resources.
- Do not stop, start, reboot, or terminate instances.
- Do not access Secrets Manager or Systems Manager Parameter Store values.
- Do not retrieve credentials, access keys, passwords, tokens, or private keys.
- Do not change IAM users, roles, policies, or permissions.
- Do not access object contents from S3.
- Do not execute shell commands other than the explicitly listed read-only AWS commands.
- Do not attempt privilege escalation.
- If a requested action requires write access, stop and report that it is outside the skill scope.

## Output

Return a concise report containing:

- AWS account ID
- IAM identity ARN
- AWS region
- Number of EC2 instances and their basic states
- CloudWatch log group names

Do not include credentials or secret values in the report.
