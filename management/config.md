# AWS Config

See [Config](/management/config.md)

## Things to Consider

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

## Features

AWS Config is designed to help you find these answers
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
