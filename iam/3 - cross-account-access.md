# Cross Account Access

## Definitions
- **Trusting** acocount has resources that need to be accessed (i.e. has the permissions)
- **Trusted** account contains user(s) that need to access resources in the _trusting_ account

## Setting up CAA
Say for example a developer needs read-only access to a service in the production environment. There is a production account, but this account is heavily restricted.
- The production account would be the _trusting_ account
- The developer's account would be the _trusted_ account

1. Create a role for the _trusting_ account (prod) and specify which permissions you want to give to the _trusted_ account (dev).
	- We can call this the "trusting role"
2. In the trusted account (dev), grant permission to allow the (dev) user to assume the newly created role (the role created by trusting/prod account)
	- Specifically grant the `sts:AssumeRole` action. Example is below:
	```
	{
		"Effect": "Allow",
		"Action": "sts:AssumeRole",
		"Resource": "arn:aws:iam::TRUSTING_ACCOUNT_ID:role/TRUSTING_ROLE"
	}
	```
3. Finally, test the cross account access by logging in to your trusted account, and then switching the role to the trusting role.
