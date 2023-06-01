# EC2 Container Service (ECS)

Allows you to run Docker-enabled apps as containers across a cluster of EC2 instances
- You don't have to manage or administrate a cluster management system because this responsibility is passed over to AWS Fargate
- **AWS Fargate**: engine that enables ECS to run containers without having to provision and manage instances and clusters for containers

## Launching an ECS Cluster
- Fargate Launch/**Serverless** Launch: You specify the CPU and memory needed, define networking and IAM policies, and package your apps into containers
	- Requires much less configuration
- EC2 Launch/**Servered** Launch: Much more configuration, you are responsible for patching and scaling instancecs, can specify instance type, and how many containers should be in the cluster

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

## Task Definitions

**Task definitions** are needed for ECS to know the requirement of your containers; including (but not limited to)
- Which docker image to use for the containers
- How much memory to allocate
- Which ports to expose
- How to configure network interfaces

## ECS Services

**ECS Service** allows you to manage, run, and maintain task definitions within an ECS cluster.
- Helps deploy your task definitions, in case any fail during their lifecycle, which allows for **self-healing containers**
- Allows you to place your containers behind an ALB

## Serverless vs Servered Launches/Containers

Cost
- Serverless compute is better when you're not utilizing all CPU or memory
- Servered compute is better when you're using up all allocated resources

CPU and Memory Utilization
- Servered is great when you have long-running workloads that fully utilize CPU and RAM

Maintenance
- Serverless/Fargate will take care of patching instances and maintaining them
- Servered/EC2 shifts maintenance responsibility to developers
	- But this also gives developers the ability to fully configure the environment of their containers

Networking
- Servered compute supports
	- **Host mode** - ties networking of container directly to it's host
	- **Bridge mode** - when using a virtual network bridge to create a layer between the host and container's networking
		- Allows you to expose ports, statically or dynamically
	- **AWSVPC mode**
		- ECS creates and manages an ENI (elastic network interface) for each task
		- Each task receives its own private IP within the ENI
		- ENI is sperate from host's ENI
		- If an EC2 instance is running multiple tasks, then each task's ENI is also separated
- Serverless only supports **AWSVPC mode**
