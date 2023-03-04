# Compute

## EC2

### Components

#### Amazon Machine Images (AMIs)
- Pre-configured templates of EC2 instances (for quick launches)
- A baseline with an operating system and some applications pre-installed
- Can create your own AMIs to speed up iterative deployments

#### Instance types
- Instances can be configured with different performance capabilities (vCPUs, type of processor, clock speed, RAM, network performance, storage)

#### Instance Families
- Micro: low-cost, low network bandwidth, low performance
- General Purpose: balanced mix of cpu, memory, and storage
- Compute Optimized: Higher ratio of cpus to memory, lowest cost per cpu
- GPU: for graphics-intensive workloads
- FPGA: provides field programmable gate arrays for specific hardware accelerations (high cpu, large memory, high network bandwidth)
- Memory Optimized: higher ratio of memory to cpus, lowest cost per GB of RAM
- Storage Optimized: ssd-backed, high I/O, low latency

#### Instance Purchasing Options
On Demand
- Launched at any time
- Can use as long as needed
- Flat rate based on instance type
- Typically for short-term use
- Best for test and dev envs

Reserved
- Purchase for a set period of time at reduced cost
- Can pay all upfront (for 1 or 3 years at a time), partial upfront, or no upfront
	- Paying more upfront gives bigger discounts
- Best for long-term, predictable workloads

Scheduled
- Reserve on a recurring schedule (i.e. daily, weekly, monthly)
- Even if you don't use the resource, you are still charged

Spot
- Bid for unused EC2 compute resources
- No guarantee for fixed period of time
- Prices fluctuate based on supply/demand
- Can purchase very large instances at low prices
- Best for processing data that can be suddenly interrupted

On-Demand Capacity Reservation
- Reserve capacity based on some attributes (i.e. instance type, platform, tenancy) within a particular AZ for any period of time
- Can be used in conjuction with the discount from reserved instances

#### Tenancy
Relates to the underlying host that your EC2 instance resides on

Shared Tenancy
- Resource launched on any available host with the required resources
- Same host can be used by multiple customers
- AWS prevents one instance from accessing another in the same host

Dedicated Instances
- Hosted on hardware that no other customer can access
- Usually needed to meet compliance
- Incurs additional charges

Dedicated Hosts
- Additional visibility and control on the physical host
- Allows to use the same host for a number of instances
- May be needed to meet compliance

#### User Data
Allows you to enter commands that wil run during the first boot cycle of that instance

#### Storage Options
Two types: persistent and ephemeral

Persistent Storage
- Provided by attaching EBS volumes
- EBS volumes are separated from EC2 instances (so you can detach a volume from one instance and attach it to another)
- Volumes are **logically** attached via AWS network
- Volumes support encryption and can take backup snapshots

Ephemeral Storage
- Created by EC2 instances using local storage
- **Physically** attached to the underlying host
- When instance is stopped or terminated, all data is lost.
	- However, if you reboot, data remains intact
- Cannot detach ephemeral storage

#### Security
- Configure a **security group** for the instance, which is basically an "instance-level" firewall that allows inbound and outbound traffic
- Create/use a cryptographic key-pair in order to encrypt login information, as well as for ssh connection
	- The public key is held by AWS. you hold the private key, and **it is your responsibilty to keep it safe**
- Customer is responsible for maintaining and installing latest OS and security patches on their instance

## Elastic Container Service (ECS)

Allows you to run Docker containers across a cluster of EC2 instances
- Managing the cluster is handled by **AWS Fargate**

**AWS Fargate** is an engine that enables EC2 to run containers, without having to manage and provision instances and clusters for the containers

A **container** holds everything an application needs to run from within with container package

Can launch an **ECS cluster** in two ways
1. Fargate Launch - you must specify the CPU and RAM required, networking and IAM policies, and packaging your app into containers
2. EC2 Launch - responsible for patching and scaling instances, specifying instance type, and how many containers are in a cluster

An **ECS cluster** is a collection of EC2 instances
- These instances function in the same way as a typical instance
	- Features such as security groups, elastic load balancing, and auto scaling can be used with these instances
- Clusters act as a resource pool, aggregating hardware resources
- Dynamically scalable and multiple instances can be used
- Can only scale in a single reigon
- Containers can be scheduled to be deployed across your cluster
- Instances within a cluster also have a Docker daemon and an ECS agent, which together allow ECS commands to be translated into Docker commands
