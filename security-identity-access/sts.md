# Security Token Service (STS)

Enables you to request temporary, limited-privilege credentials for IAM users or federated users
- Supports users federated through SAML, OAuth, OIDC, and AWS Cognito
- Accessed through global endpoint `sts.amazonaws.com`
- `AssumeRoleWithSAML` returns temporary credentials for users authenticated via SAML
- `AssumeRoleWithWebIdentity` returns temporary credentials for users authenticated through OAuth or OIDC
- All authenticated _and_ non-authenticated requests to STS endpoints are logged to CloudTrail
