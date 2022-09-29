# Identity Federation
Allows you to access/manage AWS resources if you don't have a user account within IAM
- Identity providers allow users to access securely
- Can also use any OpenID Connect provider
- Benefits of using an ID provider
	- Reduces amount of administration within IAM
	- Allows for SSO

A common example of id federation is employees accessing AWS from their company account that's managed by Microsoft Active Directory

A trust relationship must be established between the IDP (identity provider) and your AWS account

AWS supports two types of IDPs
- OpenID providers
- SAML providers

## Flow for Active Directory Authentication
1. User authenticates against Active Directory Federated Service (ADFS)
2. If successful, SAML issues authentication assertion back to user
3. User sends this auth assertion to AWS STS (Security Token Service).
	- This allows user to assume a role within IAM
4. STS responds with temporary security credentials for an assumed role
5. User now has access to AWS services
