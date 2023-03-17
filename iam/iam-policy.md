# IAM Policies

Managed Policies
- Can be attached to **multiple** users, groups, and roles
- 2 Types of managed policies
	- AWS Managed Policies: preconfigured by AWS, can copy and edit them
	- Customer Managed policies: configured by you!

Inline Policies
- Directly embedded into **one single** user, group, or role
- Do not show up under "Policies list" in AWS because they aren't publicly available for other identities
- Typically used when you don't want to risk those permissions being used by other identities
- If an identity has an inline policy, and that identity is deleted, then the inline policy is also deleted

## Example Policy
```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Sid": "Stmt123456789", // Statement id
			"Action": "s3:*",
			"Effect": "Allow",
			"Resource": "arn:partition:service:region:account-id:resource",
			"Condition": {
				"IpAddress": {
					"aws:SourceIp": "10.10.0.0/16"
				}
			}
		}
	]
}
```

## Types of Policies

Identity-Based Policies
- Managed policy vs Inline policy

Resource-Based Policies
- Associated with a resource instead of an entity (like a user or group)
- Requires a **principal**, which specifies which identities the policy applies to

Permission Boundaries
- Can be associated with a role or user, but it _does not_ grant permissions
- Defines the maximum level of permissions, or _boundaries_, that can be granted
- Examples
	- Alice has full access to S3 and RDS. However, a permission boundary restricts access only to RDS.
		- Result: Alice has full access to RDS and no access to S3
	- Alice has read/write access to S3 and read access to RDS. A permission boundary restricts access only to read from S3 and full access to RDS.
		- Result: Alice has read access to S3, and read access to RDS
		- Notice how Alice is not granted _full_ access to RDS; she is only granted read access. This is what is meant by "permission boundaries do not grant permissions"

Organization Service Control Policies
- Used by AWS organizations and attached to AWS accounts or organizational units
- Acts similarly to **permission boundaries** except it operates the the account level and affects all members of that account

## Policy Evaluation Logic
1. Authentication - is user authenticated
2. Context - what service or action is requested
3. Policy Evaluation
4. Result - whether access is allowed or denied

Policies are evaluated in this order based on their type:
1. Organizational Service Control Policies
2. Resource-based Policies
3. IAM Permission Boundaries
4. Identity-based Policies
