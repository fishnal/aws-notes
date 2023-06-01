# Identity Federation
Allows you to access/manage AWS resources if you don't have a user account within IAM
- Identity providers allow users to access securely
- Can also use any OpenID Connect provider
- Benefits of using an ID provider
	- Reduces amount of administration within IAM
	- Allows for SSO

A common example of identity federation is employees accessing AWS from their company account that's managed by Microsoft Active Directory

A trust relationship must be established between the IDP (identity provider) and your SP (service provider)

AWS supports the following IDPs
- AWS SSO - single-sign on approach that uses a single identity provider for _all AWS accounts_ within an AWS organization
- AWS IAM - allows you to configure _different_ OIDC or SAML identity providers for _each AWS account_
- Amazon Cognito - uses SAML 2.0 and web identity federation
- OpenID providers
- SAML providers

## Definitions

**OAuth 2.0** - open standard for access delegation
- Commonly used for users to grant websites/apps access to info on other services, without exposing credentials for these other services

**OpenID Connect** - identity layer on top of OAuth 2 protocol
- Enables clients to verify identity of an end-user based on authentication performed by an authorization server
- Enables clients to obtain basic profile info about end-user

**SAML 2.0** - standard for exchanging authentication and authorization identities between domains
- XML based protocol
- Uses security tokens containing assertions to pass information about a principal (typically the end-user) between a _SAML authority_ (identity provider) and _SAML consumer_ (service provider)

## Flow for Active Directory Authentication
1. User authenticates against **Active Directory Federated Service (ADFS)**
2. If successful, SAML issues authentication assertion back to user
3. User sends this auth assertion to **AWS STS (Security Token Service)**.
	- This allows user to assume a role within IAM
4. STS responds with temporary security credentials for an assumed role
5. User now has access to AWS services

## Security Token Service (STS)
STS allows you to request temporary, limited-privilege credentials for both IAM users and federated users
- By default, STS endpoints are activated in all regions. It is recommended to deactivate unused regions
- There is a global endpoint for STS, but regional endpoints can reduce latency
