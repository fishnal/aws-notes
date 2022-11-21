# RDS Multi AZ

Multi AZ means Multi Availability Zone
- Helps with resiliency and business continuinity by providing failover options (hence the word MULTI)
- In RDS, it will configure a secondary RDS replica instance within the same region as the primary, but in a different AZ

Data replication is **synchronous**

RDS uses failover mechanism on Orcale, MySQL, MariaDB, and PostgreSQL
- Failover happens automatically and managed by AWS
- Updates DNS record to point to secondary instance within 60-120s
- Failover occurs if:
	- Patching maintenance
	- Host failure
	- AZ failure
	- Instance rebooted with failover option
	- DB instance class is changed
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
