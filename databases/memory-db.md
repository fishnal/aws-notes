# MemoryDB for Redis

In-memory data store service, compotable with the **Redis data structure store**

Provides low-latency, super fast access to data (since it's stored in memory)
- Microsecond reads
- Single-digit millisecond writes

Gets **deployed as a cluster** within a VPC

Each cluster consists of **one or more nodes** that, together, serve a single dataset

**Dataset is partitioned** into **shards**

Each **shard** will have a primary node, and optionally up to 5 read replica nodes
- Can spread these replica nodes across AZs to provide higher availability

Built-in failover ability: for a shard, replica nodes can be promoted to a primary node if needed

Supports
- Clusters up to 500 nodes in size (which could offer up to 100TB storage)
- Transaction logs are distributed across AZs for durability and consistency
- Data tiering -- allows you to move data that is accessed less frequently to a persistent disk
- Encrypt data in transit and at rest
- Snapshots for easy backups and restores