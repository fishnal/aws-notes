# AWS Batch

- Batch computing is used when a lot of compute power (across a cluster of resources) is required
- With AWS Batch, AWS will manages the administration behind batch computing (i.e. scaling the compute resources)

## Components

**Jobs** - a unit of work run by AWS Batch
- Can be an executable, an app within ECS, or a shell script
- Run on EC2 instances as containers
- Have various states (submitted, pending, running, failed)

**Job Definitions** - defines parameters for the jobs, how the job is run, and what configuration is used for the job
- Configuration can include # of vCPUs, data volumes, IAM roles, and mount points

**Job Queues**
- Jobs are placed into a queue until they run
- Can have multiple queues with different priorities
- On-demand and Spot instances are supported
	- AWS Batch can bid on your behalf for Spot instances

**Job Scheduling** - when a job should run and from which compute environment
- Typically FIFO, but obviously considers higher priority queues first

**Compute Environments**
- Two types of compute environments: managed and unmanaged
- Managed Environment
	- AWS handles provisioning, scaling, and termination of resources
	- Created as an Amazon ECS Cluster
- Unmanaged Environment
	- You handle the management
	- Way more customization, but obviously more administration
	- Requires _you_ to create the ECS cluster
