# Elastic Container Registry (ECR)

- Provides a secure location to store and manage docker images
- Primarily for developers pushing/pulling docker images

## Components
1. Registry
	- Store docker images
	- For images you push, you also have read/write access
	- Can control access via IAM policies or **repository policies**
2. Authorization Token
	- Before your docker client can access registry, it must be _authenticated as an AWS user_ via an **auth token**
	- In order to authenticate as an AWS user, must login to AWS via `aws ecr get-login --region <> --no-include-email`, which will output a `docker login` command for you to execute
3. Repository
	- Objects within registry that group together and secure multiple docker images
	- Can create multiple repos
	- Can control access via IAM policies or **repository policies**
4. Repository Policy
	- These are **resource-based policies**
	- If an AWS user needs access to the registry, they must have access to the `ecr:GetAuthorizationToken` action
5. Image
	- These are docker images
	- Once authenticated, you can use familiar docker commands (docker push, docker pull)

## Lifecycle Policies

Can configure lifecycle policies for container images stored in your registry
- For example, only keeping 5 of the newest image versions, and deleting older ones
