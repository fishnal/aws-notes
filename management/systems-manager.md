# Systems Manager (SSM)

SSM is a set of fully-managed services that enables automated configuration and ongoing management of systems
- Systems can be EC2 instances, servers in your own data center, or from another cloud platform

Features
- Define automation tasks to perform on your server instances
- Define maintenance windows
- Create/update system images
- Collect software inventory
- Apply system or application patches
- Configure Linux and Windows OS
- Manage the state of your instances

Optimized native integration with other AWS management tools
- IAM
- Cloudtrail
- CloudWatch Events for event-driven automation
- And more

## Pricing

Most functionality is free

## Resource Groups

You can **group AWS resources** in Systems Manager
- Resources must be in the same region

Can select instances through following mechanisms
1. Manual
   - Manually select each instance that you want to add to the group
2. Query-based
    - Can batch select based on tags, or choose an existing Resource Group that includes your instances

Can create dashboards with resource groups

## SSM Agent

EC2 instances can be managed by SSM Agent
- Installed by default on Amazon Linux AMIs and Amazon Windows AMIs
- If installing on your own data center or in another cloud provider, you must create a **hybrid activation**
    - You are supplied an activation code and ID from the AWS console, which you supply to your SSM Agent's installation.

SSM Agent manages the following OS
- Windows Server 2003 or later
- Amazon Linux
- Ubuntu
- RHEL
- SUSE
- CentOS

Managed Instance Roles
- SSM Managed EC2 instances require a role (w/certain permissions) in order to
  - Interact with the SSM agent
  - Make the instance visible to [**SSM Fleet Manager Console**](#fleet-manager)
- AWS provides some pre-defined managed policies to make it easier for you

## Fleet Manager

All managed instances are displayed in this console

Gives visibility into details of each instance

Can connect to an instance using the **Session Manager feature**

### Session Manager

Lets you connect to any managed instance using an interactive browser shell
- Does not require any inbound ports
- Do not need to manage bastion hosts or Secure Shell keys
- Do not need Secure Shell clients

Communication between Session Manager and the instance is encrypted using TLS 1.2
- Security can be increased using a KMS key

Tracking
- Tracks all commands and outputs in a session
- Provides full logging and session audititing activity that can be sent to CloudTrail, CloudWatch, or S3

Can control which users can access specific instances via IAM policies

## Run Command

Run a command on one or more instances

Commands are defined via **documents**, which define the actions that the agent performs on your instances
- Accepts input parameters
- Essentially runs a shell for you
- Written in JSON/YAML
- Documents are shared resources within the Systems Manager console

**Rate control** - limits how many instances are concurrently executing the document
- Good if you have lots of instances, and want 4 instances to execute simultaneously

Outputs can be written to S3 bucket or to CloudWatch Logs

## Parameter Store

Centralized storage to manage configuration in plain-text data, such as
- Database connections
- License codes
- Secrets or passwords
- Any other app config data

Integrates with KMS for encrypting values

Improves overall security of your implementation by
- Allowing you to separate secrets and configuration from your application code
- Integrating with CloudTrail for auditing purposes

Other features
- Versioning
- Notifications when a parameter changes
- Create custom validation routines via Lambdas

Integrates with Systems Manager Run Command for passing in parameters

## Maintenance Windows

**Maintenance window** is a *resource* that allows you to define complex tasks via
- Run Command Documents
- Step Functions
- Lambdas

Can use `cron` or `rate` expressions to define schedules

## Patch Manager

Automate patching managed instances
- Can apply patches to the instance's OS and apps running on the instance
- Can patch entire fleets

Can generate **patch compliance reports**

Integrates with CloudTrail, Event Bridge, S3, AWS Config, and IAM

Uses a resource called **patch baselines**, which defines what your patch is doing.
- AWS has pre-defined patch baselines
- Can make your own custom baselines

**Patch groups** - for designating a group of instances that are to be patched by a specific baseline
- Helps organize  your patch deployment strategy across different environments
- Can create patch groups using the tag key "Patch Group" (case-sensitive)
  - Create a **patch group resource tag** whose key is "Patch Group" (case-sensitive)
  - Assign a value to this tag
  - Then, for any resource that you want included, add this exact key-value tag to that resource

Can define maintenance windows for patch groups

## State Manager

Allows you to control how configurations are applied
- For example, firewall settings like ports that need to be shut down

Can be used to enforce compliance

Define **State Manager Policies** using **automation documents**
- AWS pre-defined documents and custom documents

**State Manager association** is a configuration assigned to your managed instances. It defines the state that you want to maintain on those instances
- 3 parts of an association
  - A document that defines the state and what should be done, including optional runtime parameters
  - Target instances to apply the state to
  - A schedule

Event Bridge Integration
- State Manager is both an event type and a target type for EventBridge rules

Outputs of commands can be sent to S3 or CloudWatch Logs

Native integration with IAM
