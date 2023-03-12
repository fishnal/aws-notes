# RDS Multi AZ

Multi AZ means Multi Availability Zone
- Helps with resiliency and business continuinity by providing failover options (hence the word MULTI)
- In RDS, it will configure a secondary RDS replica instance within the same region as the primary, but in a different AZ
	- Note: This secondary replica is **not** a read-replica. It does not help with read throughput. It's only purpose is to be a failover for the primary instance.

Data replication is **synchronous**

RDS uses failover mechanism on Orcale, MySQL, MariaDB, and PostgreSQL
- Failover happens automatically and managed by AWS
- Updates DNS record to point to secondary instance within 60-120s
- Failover occurs if:
	- Primary instance is undergoing patching/maintenance
	- Host failure on primary instance
	- AZ failure of primary database
	- Primary instance reboots with failover option
	- DB instance class on primary database is changed (i.e. switching from general purpose to memory optimized)
- When a failover occurs, an event is recorded as `RDS-EVENT-0025`.
	- You can configure RDS to notify you via SMS or SNS

## SQL Server Mirroring

Multi AZ on SQL Server is a little different, but largely the same.

Achieved by using "SQL Server Mirroring"

Both primary and secondary instances use the same endpoint. When an outage occurs, the the physical network address fromt he failed instance is transitioned over to the mirrored instance

### Configuring environment for SQL Server Mirroring
- Must have a DB subnet group with at least 2 different AZs
- Specify the AZ that mirrored instance will reside in (obviously must be different than the primary instance's AZ)

## Amazon Aurora DB

Aurora DB clusters are fault tolerant by default. (Note: This does not mean Multi-AZ is enabled by default)
- Achieved by replicating the cluster's data across instances in different AZs

Aurora can provision and launch a new primary instance, can take up to 10 mins
- If you enable Multi-AZ, then RDS provisions a replica in a different AZ. This replica can then be promoted to primary for failovers, and will not take the 10 mins
- Can create up to 15 replicas, each with a different priority

## Read Replicas in RDS Multi-AZ
How a read-replica is created:
1. A snapshot is taken from your secondary instance. This is done to reduce the workload on the primary instance and any other read replicas.
2. The read replica instance is created from this snapshot
3. Read-only traffic can now be directed to the read-replica.

Couple of Notes
- Read replicas are updated asynchronously
- Read replicas are only available for MySQL, MariaDB, and PostgreSQL
- Can deploy read replicas in different regions, which can further improve data resiliency
- Can be promoted to primary instance
- If primary instance undergoes maintenance, then read traffic can still be served via the read-replicas

### Read Replicas on MySQL
- Supported on MySQL 5.6+
- Retention value of automatic backups of the primary instance needs to be set to 1 or more
- Can only replicate when using an "InnoDB storage engine", which is transactional
- Can have nested read-replica chains
	- You have a source database "A" and then a read replica "R1"
	- You can then create another read-replica from "R1", and so on.
	- This chain has a max depth of 4 layers (so `Source -> R1 -> R2 -> R3`)
	- Same prerequisites apply to nested read-replicas
	- Maximum of 5 read-replicas per source database
- If an outage occurs with primary instance, RDS redirects the source of the read-replicas to point to the secondary database. This ensures that async replication of data continues.
- CloudWatch can monitor synchronisation between source DB and read-replicas through a metric called `ReplicaLag`
	- Note: You want `ReplicaLag` to be as close to 0 as possible

### Read Replicas on MariaDB
- Supported on ANY version of MariaDB
- Backup retention msut be greater than 0
- 5 read replicas per source DB
- Nesting read-replicas rules apply (see "Read Replicas on MySQL")
- CloudWatch keeps track of `ReplicaLag`

### Read Replicas on PostgreSQL
- Supported on PostgreSQL 9.3.5+
- Backup retention msut be greater than 0
- 5 read replicas per source DB
- The native PostgreSQL streaming replication is used to handle replication and creation of read-replicas
- A special role is used to manage replication. However, this role does not have permissions to modify the data being transmitted
- Can create a Multi-AZ read-replica
	- When you create a read replica, RDS will automatically make a secondary read replica in a different AZ
	- This secondary read replica can also be created _even if_ the first read-replica or the source database is not Multi-AZ. This means your read replicas can be more resilient than your source DB.
- Nested read-replicas **are not supported**
- CloudWatch keeps track of `ReplicaLag`
