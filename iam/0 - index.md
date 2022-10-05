# Identity and Access Management (IAM)

## Key Notes
* Global service

## Overview
Identity Management refers to authenticating your identity (and verifying it somehow, like a username and password).
- Authenticating is verifying who you are

Access Management refers to authorization and access control
- Authorizing is verifying you are allowed/denied to do something

## Definitions
- Authenticating is verifying who you are
- **Authorizing** is verifying you are allowed/denied to do something
- **Users** are _objects_ within IAM
	- Can be a real person
	- Can be an account used only by an application (i.e. your REST server needs read access to your DynamoDB table)
- **Groups** are _objects_ that contain multiple users
	- Cannot be used for authenticating (so you can't "login as a group")
	- Used for authorizing access
	- At most 100 groups can be made
	- A single user can be in 10 groups at most
- **Roles** allow identities and services to assume a set of permissions
- **Policy Permissions** define _what_ resources can be accessed
	- By default, all actions are denied.
	- Access is only granted from an explicit allow
	- Access is denied if there are any statement that explicitly denies, and this will override any and all allows
- **Access Control Mechanisms** define _how_ a resource can be accessed

## Groups
Prefer assigning permissions to groups instead of to individual users
- Easier to grant/deny permissions for multiple users

## Roles
Using with services:
- Scenario: EC2 instance need to read from a DynamoDB table
	- Bad: Could provide credentials of another user that has read access to the table
	- Good: Make a role that has read access to the table, and assign it to the EC2.
- Scenario: Multiple EC2 instances need read access to the table
	- Assign the same role to all of these instances
- Scenario: Multiple EC2 instances now need write access to the table
	- Just modify the role to grant write access

Using with IAM users:
- Scenario: A user needs temporary access to some AWS resources
	- Bad: Assign the user to a group that has the access and remove them later, OR give the user the access and remove it later.
	- Good: Allow the user to temporarily assume a role with the access

Types of Roles:
1. Service Role (EC2, Lambda, etc)
2. Service-Linked Role: preset and immutable roles that perform a specific task
3. Roles for Cross-Account Access
	- Allows access between two AWS accounts that you own
	- Allows access between a 3rd party AWS account and one that you own
	- **Trusting** acocount has resources that need to be accessed (i.e. has the permissions)
	- **Trusted** account contains user(s) that need to access resources in the _trusting_ account
	- See additional document for how to setup cross account access
4. Role for Identity Provider Access
	- Can grant web access to identity providers (such as Amazon Cognito, Facebook, Google)
	- Can grant web access for single sign on to SAML providers
	- Can grant API access to SAML providers
	- _Note:_ Web access is the AWS console, API access is using actual API calls and SDKs.
