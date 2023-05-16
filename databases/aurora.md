# Amazon Aurora

Aurora is a fully-managed database service with superior MySQL and PostgreSQL engines
- Aims to separate the compute layer and storage layer (this means computation like queries are decoupled from the storage layer)
- Because compute and storage layers are separted, read replicas can easily be added/removed at any time

## Multi-AZ Architecture

- Aurora stores data in 10 GB blocks
- Each block is replicated 6 times across 3 AZs (total 2 per AZ)
	- By design, Aurora is only supported in regions with at least 3 AZs
- Aurora can handle up to 3 copies lost for reads, and 4 copies lost for writes
- Storage layer is presented as a _single logical volume_, which is shared across all the compute instances. This allows read replicas to achieve near-identical query performance as the master replica

## Data Management

In RDS, data replication must be done from the master to create a replica

In Aurora, there is no need for replication, because every compute instance shares the _same logical volume_

## Data Consistency

Uses a **quorum and gossip protocol** in the storage layer to ensure data is consistent
- Peer-to-peer communication ensures data is copied across each of the 6 storage nodes
	- For reads, a quorum of 3 is needed for confirmation
	- For writes, a quorum of 4 is needed for confirmation
- Protocol provides a continuous, self-healing mechanism for the storage layer

If a storage node goes down, then when it comes back online, it will receive all data modifications from its peers via the protocol
- Since AZs have high-speed interconnections, this process is very fast

If the master replica goes down, then an existing read replica is promoted to the role of master OR it will launch a replacement master
- Latter option _is not preffered_ since it takes longer

## Connection Endpoints

1. Cluster Endpoint - points to current master database instance; can read/write
2. Reader Endpoint - a load balancer that points to all read replicas; can only read
3. Custom Endpoint - a load balancer that points to specific instances you choose
4. Instance Endpoint - a specific instance within the database cluster

The endpoint load balancers are implemented via Route 53 DNS

In client layer, do not cache the _connection endpoint lookups_ longer than their specified TTLs

## Setups

**Single Master and many read replicas**
- 1 Master replica with up to 15 read replicas

**Master-Master Pair**
- Configure a pair of masters in an active-active, read-write mode
- Can be scaled up to a total of 4 masters
- Can read/write to any of the masters, thus increasing fault tolerance within the compute layer
- If one master instance goes down, then incoming write requests can be redirected to the other master instance.
- Cannot add additional read replicas to a "multi-master" Aurora cluster

## Specifications

- Data replication is asynchronous and within milliseconds
- Read replicas can be deployed in different AZs within the same VPC
- Read replicas can be deployed as _cross-region replicas_
- Read replicas can be tagged with a priority label, indicating which one is prioritzed for getting promoted to the role of master
- Aurora clusters are stopped and started in its entirety -- **all underlying compute instances** are either stopped or started.
- Daily backups with default retention of 1 day (max retention of 35 days)
- On-demand manual snapshots can be done at any time, which are stored indefinitely until you explicitly choose to delete them.
	- Restoring a manual snapshot is done on a new database
