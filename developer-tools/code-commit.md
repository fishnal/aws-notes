# CodeCommit

Fully managed git-based source control service
- Fully managed and serverless
- Designed with same features that you would expect from a git-based source control system (i.e. pushing, pulling, adding to stage, committing)
	- Also follows same terminology as most other SCMs

Integrates with CodeBuild, CodeDeploy, and CodePipeline

## Connecting to Repository

Can connect to a repo via HTTPS, SSH, or HTTPS GRC

1. Connecting via HTTPs
	1. Convenient because port 443 is open in most firewalls
	2. Ensure your IAM user has appropriate credentials by attaching the CodeCommit managed policy to the user/group
	3. **Generate an HTTPS git credential** in IAM console
2. Connecting via SSH
	1. Not all firewalls have port 22 open for security reasons
	2. Must generate key-pair and upload public key to your IAM user
3. Connecting via HTTPS GRC (`git-remote-codecommit`)
	1. When you're using temporary credentials, like federated credentials or ones from an Identity Provider
	2. Must install Python and install python package `git-remote-codecommit`
	3. Then clone repo with `git-remote-codecommit`

## Pricing

Based on number of active users in your repositories

Each active user gets allowance of
- 10GB/month of storage
- 2000 git requests/month
- If allowance is exceeded, then charged
	- per GB of storage
	- per git request

## Triggers, Notifications, and Approval Rule Templates

Receive **notfications** of _repository_ events like new commits, status changes, merged pull requests, and more
- Can be notified through **AWS Chatbot**, which can send messages to common chat services like Slack, or to AWS SNS
- Notifications can have all details (full) or just the event name (basic)

Can defined **triggers** based on _repository_ events
- Example: Execute a lambda when a branch is created
- Note: Notifications cannot trigger a Lambda directly
- Does not use cloudWatch Events rules to evaluate repository events
- Triggers are much more simplistic than notifications
	- If you need more functionality when events are triggered, can leverage the fact that CodeCommit integrates with EventBridge
	- Example: When a new commit is pushed, your trigger can integrate with EventBridge, which can then integrate with CodePipeline to kickoff a pipeline for the new commit

**Approval rule templates** are useful when approving pull requests
- Can define _how many_ approvals are needed
- Can define _who_ needs to approve
- Can define rule templates based on the branch
