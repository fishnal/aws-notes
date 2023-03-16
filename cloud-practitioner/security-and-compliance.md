# Security and Compliance

## AWS Artifact

AWS Artifact is a dashboard that provides
- Security and compliance reports known as **audit artifacts** (useful for auditing purposes)
	- Audit artifacts can be shared with auditors and regulators by creating IAM users with an associated identity-based policy that grantes access only to the necessary reports
- View, download, accept, and terminate legal agreements between you and AWS, at both the account and organization level
- Remember that AWS guarantees security _of the cloud_, whereas you must guarantee security of your applications _in the cloud_

## Identity and Access Management (IAM)

### Core Components

**Identity Management**
- Defines who you are, such as your username
- Verifying who you are, such as with a password, 2FA, security key, etc

**Access Management**
- Determines what actions a particular identity can perform (i.e. reading a database, writing to a S3 bucket)

**Access Control**
- Mechanism of accessing a secured resource
- Secured via
	- Username and password (authenticate and verify)
	- Multi-Factor Authentication
	- Federated Access - where users external to AWS can still acess resources without having to supply AWS user credentials. Instead, credentials are fetched from **identity providers**
		- [More info on identity federation](/iam/2%20-%20identity-federation.md)

**Definitions** (see [IAM Definitions](/iam/0%20-%20index.md#definitions))
- Users
- Groups
- Roles
- Policies
- Identity Providers and [Identity Federation](/iam/2%20-%20identity-federation.md)
- [Security Token Service](/iam/2%20-%20identity-federation.md#security-token-service-(sts))

### Access Reports

**Access Analyzer**
- Generate findings when a policy on a resource _within_ your zone of trust _allows access from outside_ your zone of trust
- Any resource that allows external access to your resources will be flagged
	- Could be an IAM role allowing cross-account access
	- Could be a bucket that allows a different account to upload objects
- Good because it helps you identify intended and unintended access

**Credential Report**
- Generate and download CSV files containing a list of all of your IAM users and their credentials
- Can check last time an account was used, when a password was last changed, if MFA is enabled

**Organizational Activity**
- Available when using AWS Organizations
- Can select an **organizational unit (OU)** or account to view its activity

**Service Control Policy (SCP)**
- Ties in with Organizational Activity
- An SCP does not grant permissions (unlike identity-based policies); instead they implement and enforces a boundary of permissions for AWS accounts
- Determines what an account **may or may not do**
- Example
	- Suppose Alice has full access to S3, RDS, and EC2 via an identity-based policy
	- Suppose there is an SCP associated with Alice's account, and the SCP denies access to S3
	- As a result, Alice only has access to RDS and EC2
