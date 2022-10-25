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
3. Repository
	- Objects within registry that group together and secure multiple docker images
	- Can create multiple repos
	- Can control access via IAM policies or **repository policies**
4. Repository Policy
	- These are **resource-based policies**
5. Image
	- These are docker images
	- Once authenticated, you can use familiar docker commands (docker push, docker pull)
