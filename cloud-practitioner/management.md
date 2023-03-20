# Management in AWS

## AWS CloudTrail

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

### CloudTrail Events

- Every API request is recorded as an **"event"**
- Multiple events are recorded within **CloudTrail Logs**
- Events contain an array of metadata, such as
	- Identity of the caller/requester
	- Timestamp of request
	- Source IP address

### CloudTrail Logs

- New log file created every 5 minutes
- Logs are stored in an S3 bucket _that you choose_
- Log files can be stored for as long as required; which allows you to look at the history of **ALL** API requests
- Log files can also be sent to **CloudWatch Logs** for **metric monitoring and alerting via SNS**

## AWS Config

**Things to Consider**
- AWS Config **does not work for every service**
- Here are services that Config _does_ support
	- Certificate Manager
	- CloudTrail
	- EBS
	- EC2
	- Elastic Load Balance
	- IAM
	- Redshift
	- RDS
	- S3
	- VPC
- AWS Config is **region-specific**, so you have to setup Config in each region that has resources you want to track
	- If you want to track resources that are global (such as IAM), then when you are setting up AWS Config, you can select an option to include global resources

Before diving into AWS Config, here are some problems regarding resource management that a business will have:
- What resources do we have? Are there any dead/unused ones?
- Are there any security vulnerabilities?
- How are the resources linked within the environment (i.e. which resources interact with each other)
- Is our infrastructure compliant?
- Do we have accurate auditing information? (which can be used for official or governmental audits)
- Do we have a history of changes in resources?

These questions _can_ be answered by leveraging monitoring tools (i.e. CloudTrail, CloudWatch). **However** when done like this, sifting through all the data to find answers is _very resource-intensive_.

**AWS Config is designed to help you find these answers**
- Captures resource changes
- Acts as a resource inventory
	- Can discover supported resources running in your environment
- Stores configuration history for individual resources
- Provides a snapshot in time of current resource configurations
	- An entire snapshot of all supported resources within a region can be captured. The snapshot will detail their current configurations along with related metadata
- Enables notifications when a resource is changed/modified
- **Integrates with CloudTrail** to help you identify who made changes and when, and with which API
- Enforce rules that check compliancy of your resources against specific controls
	- Predefined and custom rules can be configured
- Perform security analysis
	- Security resources are recorded
	- Run security checks (i.e. checking that EBS volumes are encrypted)
- Provide relationship connectivity information between resources
	- AWS management console has a relationship query, which allows you to identify relationships between resources
	- Example: When looking at an EBS volume, you can see which EC2 instance that the volume is attached to

## Amazon CloudWatch

**Global service** designed to help monitor health and performance of your resources
- Heavily used by operational roles and site reliability engineers

**Components**
- CloudWatch Dashboards
	- Can build and customize dashboards using visual widgets
	- Can be done via console, CLI, or API
	- Can include resources from multiple regions in the same dashboard
- ClouWatch Metrics and Anomaly Detection
	- Monitor a specific **metric** of an app or resource over a period of time
	- For some services, you can enable **detailed monitoring** for a metric, which will collect data at more frequent interviews
	- Can create **custom metrics** for your apps
	- **Anomaly detection** - employs machine learning against your metrics to detect activity that is unusual
- CloudWatch Alarms
	- Tightly integrates with metrics
	- Can automatically take action when thresholds for a metric are exceeded
	- 3 Alarm States
		- `OK` - metric is within threshold
		- `ALARM` - metric has exceeded threshold
		- `INSUFFICIENT_DATA` - alarm just started, metric is not available, or not enough data has been collected yet
	- Example: an alarm when EC2 instances scale in/out (horizontal scaling)
- CloudWatch EventBridge
	- [See below](#cloudtrail-eventbridge)
- CloudWatch Logs
	- Gives you a _centralized location_ to house all of your logs from different AWS services (i.e. CloudTrail, EC2, Lambda logs) and your own applications
- **CloudWatch Insights**
	- Provides ability to get more information from the data that CloudWatch collects
	- 3 types of insights
	- **Log Insights** - analyze CloudWatch logs at scale in seconds, using interactive queries and visualizations
	- **Container Insights** - aggregates and group different metrics from different container services and container applications within AWS
		- Can be analyzed at the cluster, node, pod, and task levels
	- **Lambda Insights** - aggregates system and diagnostic metrics related to Lambda
		- This must be enabled on a **per-lambda basis**

**Unified CloudWatch Agent** will collect logs and other metrics from EC2 instances and on-premise services running either Linux or Windows

### CloudWatch EventBridge

- An option for implementing event-driven architecture
- A way to connect your apps to **targets**, which allows you to implement real-time monitoring and respond to **events** that occur in your application as they happen
- An **event** is anything that causes a change to your environment or application

#### Components of EventBridge

**Rules** - filters incoming events, and routes them to the appropriate target (as defined in the role)
- A single rule can route traffic to multiple targets

**Targets** -where events are sent by rules
- Examples: Lambda, SQS, Kinesis Stream, SNS

**Event Bus** - component that receives the event from your applications
- Rules are associated with a specific event bus
- CloudWatch EventBridge is the **default** event bus
