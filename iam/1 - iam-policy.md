# IAM Policies

Types of policies
- Managed Policies
	- Can be attached to **multiple** users, groups, and roles
	- 2 Types of managed policies
		- AWS Managed Policies: preconfigured by AWS, can copy and edit them
		- Customer Managed policies: configured by you!
- Inline Policies
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
			"Resource": "*", // Which resource the statement applies to (can be a user, another AWS service)
			"Condition": {
				"IpAddress": {
					"aws:SourceIp": "10.10.0.0/16"
				}
			}
		}
	]
}
```
