# VPC Flow Logs

captures traffic flowing within VPCs

flow logs can be stored in S3 or sent directly to CloudWatch

limtations
- for vpc peered connections, can only see flow logs of peered vpcs within same account
- cannot retrieve info from resources within EC2-Classic environment
- Once a Flow Log is created, it cannot be changed. You must delete and re-create one if you want to update it.
- Traffic that is not captured
  - DHCP traffic within vpc
  - Traffic from instances destined for Amazon DNS Servers
  - Traffic destined to IP addresses for the VPC default router
  - Traffic to and from:
    - Instance Metadata service
    - Time Sync service
  - Traffic relating to an Amazon Windows activation license ONLY if it comes from a Windows instance
  - Traffic between an NLB's Network Interface and an Endpoint Network Interface

Can setup Flow Log against the following
- A network interface on an EC2 instance
- A subnet within a VPC
- A VPC itself
- For capturing traffic in VPCs and subnets, data is captured for *all* network interfaces

Process of Flow Logs sending data to CloudWatch
1. Each captured network interface will publish data to your log group using a different log stream
2. Each log stream contains event data from the Flow Log
3. Each log stream will capture traffic in a 10-15 minute window

Requirements for Flow Log to push data to your CW log group
- Role with following actions allowed
  - `logs:CreateLogGroup`
  - `logs:CreateLogStream`
  - `logs:PutLogEvents`
  - `logs:DescribeLogGroups`
  - `logs:DescribeLogStreams`
- Flow Log can assume the above-mentioned IAM role
  - Allow `sts:AssumeRole`, with the VPC Flow Log service as the principal
- User creating the Flow Log must have access to the `iam:passrole` action, which allows the service to assume the above-mentioned role

Permissions to review and assess Flow Logs
- `ec2:CreateFlowLogs`
- `ec2:DeleteFlowLogs`
- `ec2:DescribeFlowLogs`
- `logs:GetLogData`
  
Notable information within the logs
- `Action` - whether traffic was accepted/rejected by security groups and NACLs
- `LogStatus` - status of the logging with 3 values:
  - `OK`
  - `NoData` - no traffic to capture during the capture window
  - `SkipData` - some data within the log was captured due to an error, so you *might* want to skip this log
