# Elastic Compute Cloud (EC2)
EC2 deploys virtual servers

## Components of EC2
- Amazon Machine Images (AMIs)
- Instance Type
- Instance Purchasing Options
- Tenancy
- User Data
- Storage options
- Security

### AMIs
- Pre-configured instances that you can quickly launch
- AMI is an image baseline with an OS, some applications, and custom configuration
- Where to get an AMI?
	- AWS provides some of their own
	- Create your own
		- You can launch an EC2 instance, install some custom applications and configurations, and then create an AMI from this customized instance.
	- AWS marketplace, where you _**buy**_ from trusted 3rd party vendors
	- Community marketplace (created and shared by other AWS members)

### Instance Types
- Defines the size of certain parameters for your instance, such as EC2 compute units, vCPUs, clock speed, memory, and more.
- Instance families
	- **General purpose**: balance of CPU, memory, and storage
	- **Micro instances**: low cost due to low CPU and memory
	- **Compute optimized**: high performance
	- **GPU**: for graphics-related processing
	- **FPGA**: for customizing field programmable gate arrays (useful for massive parallel power, such as in genomics and finances)
	- **Memory optimized**: high memory, lowest cost per GB of RAM
	- **Storage optimized**: backed by SSDs for high IO performance and low latency
		- Great for log processing, noSQL databases

### Instance Purchasing Options
- **On demand**
	- Launched at any time, use as long as you want
	- Best for dev/test environments
	- Flat rate
- **Reserved**
	- Purchase instance for a set period of time
	- Can pay all upfront (best discount), partial upfront, and no upfront (no discount)
- **Scheduled**
	- Pay for your instance to run on a recurring schedule and/or on a set time frame (i.e. every day at 2am for 6 hours)
- **Spot**
	- Bid for unused EC2 compute resources
	- Price fluctuates based on supply/demand
	- No guarantee of resources for a fixed period of time
	- If lucky, can purchase large EC2 instances at a low price
	- Best suited for when processing can be suddenly interrupted
- **On demand capacity reservations**
	- Reserve a capacity based on different attributes (i.e. instance type, platform, tenancy) within a particular availability zone for any period of time
	- Can be used in conjuction with your reserved instances discount

### Tenancy
Describes the underlying host/machine your EC2 instance resides on
- Shared Tenancy
	- Launches instance on any available host with the requested resources
	- Same host may be used by multiple customers
	- AWS has security mechanisms to prevent one instance from accessing another instance
- Dedicated Instances
	- Launches instance on a host that _no other customer can access_
	- Incurs additional charges since there might be unused capacity on the host you use (which is capacity that other customers _could have_ used)
- Dedicated Hosts
	- Similar to dedicated instances
	- Additional visibility and control on the host

### User Data
Allows you to enter commands that run during the _first_ boot cycle
- Can pull down additional software, or update existing software
- Can download latest OS updates

### Storage Options
- Persistent storage
	- Backed by EBS volumes, which are separated from the instance
	- EBS volumes are logically attached via AWS network
	- Can connect/disconnect volume from instance
	- Can implement encryption and take snapshots
- Ephemeral storage
	- Physically attached to underlying host
		- B/c of this, you can't detach the storage volume from the instance
	- When instance is _stopped or terminated_, all data is lost
	- When instance is _rebooted_, data remains intact

### Security
- Can assign a security group
- Can define rules that allow/deny access to TCP/UDP ports
- At the end of instance creation, you must select a key pair to use (or create a new one)
	- Used to encrypt the login information for Linux/Windows instances
	- Can use the same key pair for multiple instances
- It is **your responsibility** maintain and install security patches
