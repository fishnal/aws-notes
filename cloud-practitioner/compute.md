# Compute Fundamentals

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

## Elastic Container Registry (ECR)

A secure location to store and manage your own docker images
- Primarily used by developers to push, pull, and manage their library of images

### Components of ECR

#### Registry
- Hosts and stores docker images
- Can create **image repositories**
- For any image you create, your account will have read and write access by default to those images
- Access to registry and images can be controlled via IAM and **repository policies**
- In order for your docker client to access ECR, you need to have an AWS Authorization Token

#### Authorization Token
- Can fetch token via AWS CLI using `aws ecr get-login-password`
- **NOTE:** The user must have permission to the `ecr:GetAuthorizationToken` action
- The token expires after 12 hours

#### Repository (or Image Repository)
- Groups multiple docker images together
- Can create multiple repos
- Can use IAM and repository policies

#### Repository Policy
- Resource based policies
- Determines who has access to which repositories/images, and what they can do with those objects

#### Image
- These are the docker images that you want to store

## Elastic Container Service for Kubernetes (EKS)

See [EKS](/ec2/3%20-%20eks.md)

## Elastic Beanstalk

See [Elastic Beanstalk](/ec2/4%20-%20elastic-beanstalk.md)

## Lambda

See [Lambda](/ec2/5%20-%20lambda.md)

## Batch

See [Batch](/ec2/6%20-%20batch.md)

## Lightsail

See [Lightsail](/ec2/7%20-%20lightsail.md)

# Elastic Load Balancing

## Elastic Load Balancer (ELB)

To manage and control the flow of inbound requests destined to a group of targets
- Targets could be EC2 instances, Lambdas, IP addresses, containers
- Targets can reside in different AZs, or all placed in a single AZ

ELBs can detect when an instance goes down, and re-route traffic to healthy instances accordingly

### Components of ELB

- **Listeners** - defines how inbound connections are routed to your **target groups** based on ports and protocols set as **conditions**
- **Target groups** - a group of resources that you want the ELB to route requests to based on **rules**
- **Rules** - define how an incoming requests gets routed to which target group
- **Health checks** - a health check is performed against all resources within a target group
- **Internet-facing ELB**
	- The **nodes** of an ELB are accessible via internet and thus have a public DNS name, public IP, and an internal IP
	- This allows the ELB to serve incoming requests from the internet
- **Internal ELB** - an internal ELB that has **only** an internal IP, so it can only serve requests that originate from within the ELB's VPC
- **ELB Nodes** - used by ELB to distribute traffic to your target groups
	- An ELB node is created within every AZ that your ELB is deployed to
- **Cross-Zone Load Balancing** - distributes traffic evenly between all resources in all AZs that the ELB is deployed to
	- Suppose you have 4 instances in Zone1 and 10 instances in Zone2
	- Without cross zone balancing, traffic is split equally **amongst the AZs**:
		- 50% of traffic is routed to each zone
		- In Zone1, each of the 4 instances will handle 12.5% of all requests
		- In Zone2, each instance will handle 5% of all requests
	- With cross zone balancing, traffic is split equally **amongst the resources**:
		- Since there are 14 total resources, each resource will end up handling ~7% of all requests.

![ELB components](./assets/elb-components.png)

## Using HTTPS as an ALB Listener

- You need an X509 server certificate and an associated security policy
	- This is needed to allow the ALB to decrypt the HTTPS requests in order to figure out which target group to forward the request to
- Selecting a certificate:
	- Can select from/upload to ACM (recommended)
	- Can select from/upload to IAM (only recommended when deploying ELBs in regions that are not supported by ACM)
