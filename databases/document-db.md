# DocumentDB

- Non-relational database
- Highly scalable, fast, high availability
- Runs in a VPC
- Compatible with MongoDB (either managed by AWS or self-hosted by yourself) via the **AWS Database Migration Service (DMS)**
- Automatic backups
- Automatic snapshots of storage volume daily

Storage scaling
- Increases storage size by 10GB each time (as necessary) up to a max of 64TB

## Architecture

Cluster
- 1 or more DB instances (up to 16) that span multiple AZ within a single region
- Shared cluster storage volume for all instances in cluster

Instance
- Provide the processing power
- Only 1 primary instance in a cluster that performs write operations. All other instances are read replicas
- Data is copied to read replicas synchronously
- If primary instance goes down, a read replica will be promoted to be primary

## Connecting to DBs
- Cluster endpoint
	- Points to primary instance
	- Used for read/write operations
	- If primary instance fails, then this endpoint will point to the new primary instance
- Reader endpoint
	- Connects to any read replica (AWS picks for you)
	- Only for read operations
- Instance endpoint
	- Each instance has it's own unique instance
	- Useful for load balancing
