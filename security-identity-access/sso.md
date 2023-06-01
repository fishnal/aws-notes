# AWS SSO

Helps implement a federated access control system
- Personalized portals for end-users to centrally gain access to your AWS accounts
- Integration with CloudTrail for auditing
- Integrates with AWS Organizations

## Enabling with AWS Organizations

- Organization must be created with ALL FEATURES enabled, not just consolidated billing features
- Must enable SSO from the management AWS account of the organization
- Pick an identity pool you want to use
	- Can be external such as Azure Active Directory or Okta Universal Directory
	- Can choose to use default identity store from AWS SSO
