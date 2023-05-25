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

## CloudTrail Events

- Every API request is recorded as an **"event"**
- Multiple events are recorded within **CloudTrail Logs**
- Events contain an array of metadata, such as
	- Identity of the caller/requester
	- Timestamp of request
	- Source IP address

## CloudTrail Logs

- New log file created every 5 minutes
- Logs are stored in an S3 bucket (that you choose)
	- Logs are delivered 15 mins after the API was called
- Log files can be stored for as long as required; which allows you to look at the history of **ALL** API requests
- Log files can also be sent to **CloudWatch Logs** for **metric monitoring and alerting via SNS**

Notable metadata fields in logs
- `userAgent` - agent method that the request was made through
	- `signin.amazonaws.com` comes from the login page
	- `console.amazonaws.com` comes from the console
	- `lambda.amazonaws.com` comes from a lambda

### Aggregating Logs into a Single Bucket

You can aggregate logs from multiple accounts into a single bucket (which belongs to one of the accounts)
- You **cannot** aggregate logs from multiple accounts into _CloudWatch Logs_ belonging to a single account

How to setup
1. Create a Trail that you want the logs to be sent to
2. Apply permissions to the destination S3 bucket allowing cross-account access for CloudTrail
	- Once applied, add a line for each account requiring access under the resource attribute
3. Create a new trail in the other accounts, and select the bucket as the one you used in step 1 (aka, the single bucket that we want logs in)
	- If you specified a log prefix when creating the trail in step 1, then you must specify the same log prefix for all of the other trails

Restricting users to only access logs that originated from their AWS account
1. In master account, create IAM roles that allow read access
2. Add a policy to those roles to access the relevant AWS account logs only
	- For this statement, the `Resource` attribute would be a wildcard pattern that matches for the given AWS account
	- For example, the `Resource` value might be `arn:aws:s3:::my-org-name/aws-logs/111111111111/*`, which means "only files matching this path"
3. Add a policy for each user to assume the role (created in step 1). This means each user needs access to `sts:AssumeRole` for the role created in step 1

### Log File Integrity Validation

Can enable **Log File Integrity Validation**, which allows you to verify your logs have not changed since CloudTrail delivered them to the bucket.

How file integrity is tracked
- When a log file is delivered to the bucket, CloudTrail creates a hash for that file.
- CloudTrail creates a new file every hour called a **digest file**, containing details of all logs delivered within the hour and _the hash for each file_
- Digest files are stored in same bucket as the log files, though digests are in a separate folder
- CloudTrail **signs** each digest file with a private key of a key-pair.
- After delivery, you can use the public key to verify the signature
- CloudTrail uses different key-pairs for each region.

Verifying File Integrity
- Can only be done programmatically or CLI
- CANNOT do via console

### Monitoring CloudTrail Logs with CloudWatch Logs

Can configure a trail to integrate with CloudWatch Logs, which allows any API events captured by your tail to be delivered to CloudWatch for monitoring and alerting.

Trail must have access to the following actions on your CloudWatch Log Group resource:
- `CreateLogStream`
- `PutLogEvents`

**Note: CloudWatch Log Events have a max size of 256KB; so if a CloudTrail event is larger than this, then it won't be sent to your CW log group**

After trail is setup to send events to the log group, then configure CloudWatch to analyze your trail's events within the log files by adding **metric filters**
- Allows you to search and count a specific value or term within events in a log file
- Must create a **filter pattern** that you want CW to monitor and extract from your files
