# EC2 Container Service (ECS)

Allows you to run Docker-enabled apps as containers across a cluster of EC2 instances
- You don't have to manage or administrate a cluster management system because this responsibility is passed over to AWS Fargate
- **AWS Fargate**: engine that enables ECS to run containers without having to provision and manage instances and clusters for containers

## Launching an ECS Cluster
- Fargate Launch: You specify the CPU and memory needed, define networking and IAM policies, and package your apps into containers
	- Requires much less configuration
- EC2 Launch: Much more configuration, you are responsible for patching and scaling instancecs, can specify instance type, and how many containers should be in the cluster

## Additional Notes
- **ECS is region specific**
	- Clusters can only scale in a single region
	- But, clusters can scale across multiple availability zones
- Monitoring is taken care of by CloudWatch
- Cluster acts as a resource pool
- Clusters are dynamically scalable
- Can schedule deployments for containers in a cluster
- Instances within the cluster have a Docker daemon and an ECS agent
	- Allows instances (within same cluster) to translate ECS commands into Docker commands
