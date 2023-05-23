# AWS CloudTrail

Records and tracks all API requests in your AWS account
- Example: When your EC2 instances auto scale, the api request that initiates auto-scaling is recorded
- Requests can be initiated from one of the following:
	- SDKs
	- CLI
	- AWS Management Console
	- Another AWS Service
- Is a **global service that supports ALL regions**
- Supports over 60 AWS services and features

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
