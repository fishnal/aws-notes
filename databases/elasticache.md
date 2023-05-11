# ElastiCache

In-memory caching

Supported for database services (RDS and DDB)

Can be a really cost-effective alternative compared to vertically scaling your compute power
- You can cache your results/data in-memory and still get fast responses

# Engines

ElastiCache for Memcached
- Sub-ms latency
- Key-value store based
- Can be used as either a cache or a data store

ElastiCache for Redis
- Sub-ms latency
- Can be used as either a cache or a data store
- Can be configured to use "cluster mode" (more on this below)
	- Disabled: 1 shard
	- Enabled: Up to 90 shards per cluster

Memcached and Redis mainly differ in their feature sets
- Both support caching and data store
- Redis supports more:
	- Chat and messaging
	- Leaderboards
	- Geospatial data
	- Machine learning
	- Media streaming
	- Queues
	- Real time analytics

# Components

**Node** - a fixed size of network attached RAM
**Shard** - a group of 1-6 nodes
**Redis Cluster** - a group of 1-90 shards
**Memcached Cluster** - a group of 1+ nodes (note that memcached clusters do not have shards)

# Use cases
- Gaming leaderboards that update quickly
- Storing temporary session info (such as for websites)
- Real time analytics in conjuction with other AWS services (i.e. Kinesis)

# Do not use for...
- Data persistence is needed
- Working with primary data records
- When write performance is more important than read performance
