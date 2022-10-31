# DynamoDB (DDB)

Services for NoSQL databases/key-value stores
- Look up data via primary key or through indexes
- High performance, single-digit ms latency
- Storage automatically grows, so you don't have to scale i

Pricing is based on
- Amount of storage
- Total throughput you configure (doesn't matter if you don't use it all) or on-demand throughput

Can encrypt data at rest via
- A key owned by AWS DDB (no charge)
- KMS - Customer managed CMK (a key stored in your account that you manage)
- KMS - AWS managed CMK (a key stored in your account, but managed by AWS, charges apply)

# Secondary Indexes
- If you want to query and match on 2 different columns, you need an index
- When you want to use an index, you need to explicitly mention that in your request to DDB.

**Global Secondary Index (GSI)** - query across entire table
**Local Secondary Index (LSI)** - can only find data within a single partition key

# Pros vs Cons
Pros
- Fully-managed
- Schemaless
- Highly available
- Super fast
Cons
- Eventual consistency
- Queries are less flexible than with most relational databases. This ensures all queries are as fast as possible
- Maximum record size is 400KB
- Can have up to 20 GSIs and 5 LSIs
- If you use provisioned throughput and you use up all your read capacity units, you will get an error if you make another read operation. (Same thing for write)
	- Although, you can always just provisioned throughput at any time
