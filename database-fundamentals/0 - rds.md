# Relational Database Service (RDS)

Service for relational databases
- MySQL
- PostgreSQL
- MariaDB
- Oracle
- SQL Server
- Amazon Aurora

Can select different compute instances (based on vCPU, RAM)
- _Can select between general purpose or compute-optimized_

RDS instances can reside in either a single availability zone (AZ) or multiple AZs
- When setting up instances in multiple AZs, you are still restricted to the same region as the primary instance
	- **Note:** _This restriction applies to RDS, but I don't know if it applies to AZ in general)_
- Data replication from primary instance to secondary instances happens asynchronously

In the event of an outage on your primary instance, it takes 60-120 seconds for RDS to update DNS records to point to the secondary instance.

Primary instance can go down if:
- Patching maintenance
- If primary instance has a host failure
- If primary instance's AZ fails
- If primary instance was rebooted with failover
- If primary instance class is modified

## Scaling Storage
Two types of storage: Elastic Block Storage (EBS) and Shared Cluster Storage

**EBS**
- General purpose SSD storage
	- Good for most use cases
	- Single-digit ms latency
	- Cost effective
	- Allocated Storage for primary data
		- Minimum 20GiB (gibibytes)
		- Maximum 64TiB (SQL Server max at 16 TiB)
- Provisioned IOPS (SSD) storage
	- For workloads that need very high I/O
	- IOPS
		- Minimum: 8000
		- Maximum: 80,000 (SQL Server max at 40,000)
	- Allocated Storage for primary data
		- Minimum: 100GiB
		- Maximum 64TiB (SQL Server max at 16 TiB)
- Magnetic Storage
	- Intended for backwards compatability
	- Not recommended; instead prefer general purpose ssd

**Shared Cluster Storage**
- Uses shared cluster storage architecture
- Cannot configure storage options
- Storage automatically scales as your db grows

## Scaling Compute Power

Vertical Scaling - improving the hardware
- Can do this at any time for RDS
- Can apply your new changes immediately or schedule them later
Horizontal Scaling - adding more instances
- Can create **read-only replicas** of the RDS instance
- Read replicas help to offload traffic from main instance
- When creating a read replica, it will clone the data from your main instance. If you use multi AZ, then it will clone data from a secondary instance to reduce performance impacts.

## Automated Services
- Patches
- Backups
	- Automated backups will be deleted after a certain duration
	- Manual backups do not expire and can only be deleted with manual intervention
	- Amazon Aurora gives option to "backtrack" which lets you rewind the DB cluster to a specific point in time, without having to create an additional cluster
