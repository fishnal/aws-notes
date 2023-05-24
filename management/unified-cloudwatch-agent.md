# Unified CloudWatch Agent

**Unified CloudWatch Agent** will collect logs and additional metrics (ones that CloudWatch _does not_ already track for you) from EC2 instances and on-premise services running either Linux or Windows

## Setting up the agent
1. Create 2 roles (see [Setting up the roles](#setting-up-the-roles))
	- 1 with permissions to collect data from your instances and send them to CloudWatch; this role also serves to install the agent
	- 1 with permissions to store the agent's configuration file in SSM
2. Attach role to the instance
3. Download and install Unified CloudWatch agent onto EC2 instance
	- See [Download Options](#download-options) for different ways the agent can be downloaded
4. Configure and start the agent

As best practice, only one of your instances should have the SSM role.
- Once the agent configuration file is stored in SSM, there is no need for other EC2 instances to have write-access to SSM.
- Once the config file has been stored in SSM, this SSM role should be detached from the instance

### Setting up the roles

SSM Role
- Trusted identity: `AWS Service`
- Service that will use this role: EC2 Allows EC2 instances to call AWS services on your behalf
- Permission Policies to attach: `CloudWatchAgentAdminPolicy` and `AmazonEC2RoleForSSM`

Cloudwatch Role
- Trusted identity: `AWS Service`
- Service that will use this role: EC2 Allows EC2 instances to call AWS services on your behalf
- Permission Policies to attach: `CloudWatchAgentServerPolicy` and `AmazonEC2RoleForSSM`

### Download Options

Can download from public S3 links

Can download and install via Systems Manager (SSM)
- Pre-requisites
	- Instance has access to internet
	- Instance has SSM agent installed (note: some images will already have SSM installed)
- Install via a Systems Manager "Run Command" with the following options
	- Command Document: `AWS-ConfigureAWSPackage`
	- Targets: select your instances
	- Command Parameters
		- Action: Install
		- Name: `AmazonCloudWatchAgent`
		- Version: Latest

### After Installing and Configuring Agent

Now, we need to start the agent, which can be done via Systems Manager "Run Command"
- Command Document: `AmazonCloudWatch-ManageAgent`
- Targets: select your instances
- Command Parameters
	- Action: `configure`
	- Mode: `ec2`
	- Optional Configuration Source: `ssm`
	- Optional Configuration Location: Set this to whatever you saved the config file as in SSM
