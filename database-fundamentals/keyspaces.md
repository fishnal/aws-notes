# Amazon Keyspaces (for Cassandra)

- Open source
- Distributed
- Wide column
- NoSQL
- High Availability
- High Scalability
- Unlimited throughput (good for massive-scale solutions)
- Fast performance and elasticity
- Low latency

## Cassandra Architecture

Comprised of a cluster of nodes, which grows based on how the database grows

**Keyspace** - a grouping of tables that are related, and are used by your applications to read/write data.
- Defines how tables are replicated across the multiple nodes in a cluster
- This is all abstracted and managed by AWS

**Tables** are where your data is stored
- By default, new tables are encrypted at rest
- By default, clients must establish a TLS connection in order for data to be encrypted in transit

## Throughput

On-demand and Provisioned
- On-demand is best for unpredictable workloads
- Provisioned has the benefit of having faster throughput speeds than on-demand.
- Provisioned also uses automatic scaling (via lower and upper thresholds) to match demands as needed

## Cassandra Query Language (CQL)

- Query language for cassandra
- Similar to SQL
- Can use CQL editor in AWS to query your keyspace
- Can use CQLsh to query
- Up to 1000 records per query
