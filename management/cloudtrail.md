# AWS CloudTrail

Records and tracks all API requests in your AWS account
- Example: When your EC2 instances auto scale, the api request that initiates auto-scaling is recorded

Requests can be initiated from one of the following:
- SDKs
- CLI
- AWS Management Console
- Another AWS Service

Is a **global service that supports ALL regions**

Supports over 60 AWS services and features

Use Cases
- Effective for security analysis
	- Monitor restricted API calls
	- Notify of threshold breaches
- Resolve day-to-day operational issues
	- Filtering options for isolating data
	- Faster root-cause identification
- Tracking changes to your AWS infrastructure
- CloudTrail logs and be used as evidence for compliance and governance controls

## CloudTrail Events

- Every API request is recorded as an **"event"**
- Multiple events are recorded within **CloudTrail Logs**
- Events contain an array of metadata, such as
	- Identity of the caller/requester
	- Timestamp of request
	- Source IP address

## CloudTrail Logs

- New log file created every 5 minutes
- Logs are stored in an S3 bucket _that you choose_
- Log files can be stored for as long as required; which allows you to look at the history of **ALL** API requests
- Log files can also be sent to **CloudWatch Logs** for **metric monitoring and alerting via SNS**

## Components

Trails - main building block

S3 - for storing CloudTrail logs, also needed for each trail created

Logs - created by CloudTrail and records all events captured (see [above](#cloudtrail-logs))

KMS (optional) - encrypting log files at rest

SNS (optional) - notified when a new log is delivered to S3, or be used in conjunction with CloudWatch when thresholds have been violated

CloudWatch Logs (optional) - allows you to deliver logs to CloudWatch Logs as well for more specific metric monitoring

Event Selectors - adds a level of customization to the type of API requests you want a trail to capture

Events - every API request captured as an "event" in a CloudTrail log file

API Activity Filters - search filters that can be applied to your API activity history
- These events are kept in the console for 7 days, even if the trail is stopped or deleted

## CloudTrail Flow

Creating a Trail
1. Specify an S3 bucket for log storage
2. Optional - Setup KMS encryption for log files
3. Optional - Setup SNS notifications of new log files
4. Optional - Enable log file validation

After trail is created
1. Optional - Deliver CloudTrail logs to CloudWatch for monitoring
2. Optional - Configure event selectors
3. Optional - Tagging

Capturing an API Request
1. CloudTrail checks if the API call matches any configured trails
2. If matched, APO call is recorded into the current log file
3. Log file is eventually sent to S3 (and optionally CloudWatch)
