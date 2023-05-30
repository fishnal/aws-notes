# Secrets Manager

Service for storing secrets
- Removes the need to hard-code secrets, credentials, or sensitive values within your app
- Can act as a central store for secrets, unlike Systems Manager Parameter Store
- Can create read-replicas of your secrets for other regions

## Contrasts with Systems Manager Parameter Store
- Encryption is not enabled by default for parameters (because not all parameters need to be encrypted). Secrets are encrypted by default and cannot be disabled
	- Secrets are encrypted via
		- Default AWS-owned key called `DefaultEncryptionKey`
		- AWS-managed key
		- Customer-managed key
- Parameter Store does not offer automated rotation
- You can share secrets between multiple accounts; however, you cannot share parameters between accounts.
- Size of parameter/secret
	- Standard Parameter (free) - 4KB
	- Advanced Parameter (costs money) - 8KB
	- Secrets - max 10KB

## Rotating Secrets

Can rotate credentials (that are stored as secrets in AWS) automatically for the following services
- RDS
- DocumentDB
- Redshift

If you want to auto-rotate secrets for other services, then you can write a Lambda function to do that
- Under the hood, credentials for the above AWS services are rotated by Lambdas

## Pricing

- Charged for storage, but there aren't different storage tiers like Parameter Store has
- Charged for API calls
