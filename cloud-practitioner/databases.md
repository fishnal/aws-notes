# Database Types

## Database Landscape
**Relational databases**, or **SQL databases**, store data in a structured manner, based on some schema
- Optimized around data storage

**Non-relational databases** for storing data in no particular structure
- Often distributed across multiple computers or nodes

AWS offers the following types of databases
- Relational
- Key-value
- Document
- In-memory stores
- Graph
- Columnar
- Time-series
- Quantum ledger
- Search

Workload Types
- **Operational or Online Transactional Processing (OLTP)**
	- Most common today
	- Centered around processes that are regular, repeatable, and durable
	- Database is often relational since data is controlled and predictable (so fixed schemas can be appropriate)
	- Use cases include e-commerce, content management, or information management
- **Analytical or Online Analytics Processing (OLAP)**
	- Ran as needed (as opposed to regular/repeatable work)
	- Used for data analysis (duh...)
	- Workloads are often:
		- Retrospective (i.e. what happened in the past year)
		- Streaming - for gathering and analyzing data in real-time (i.e. to find trends or raise an alarm)
		- Predictive Analytics - using data to help look into the future (i.e. via ML or AI)
	- Database is often non-relational since the data is often unstructured

## Relational Databases
Relational DBs require a **schema**, which defines how your data is structured
- As a result, the database's expected output drives how the data should be stored
- Developers work backwards from the requirements to determine a schema.

Changing schemas is expensive in terms of time and compute resources; and has a risk of corrupting data (i.e. breaking schema changes) and a risk of brekaing existing reports

AWS offers the following Relational DB engines via **RDS**
- Amazon Aurora - cloud native version of MySQL and Postgres
- MySQL
- Postgres
- MariaDB
- Oracle
- Microsoft SQL Server

### Components of Relational DBs
- Data is stored in **tables** or **relations**
- Each **row** or **record** in the table has **key-value attributes**
- Every **row** has a **unique primary key**
- **Relationships** are created between tables using the **unique primary keys** (the "linking" key in a row is also called the **foreign key**)

### Data Integrity
Follows ACID
- Atomicity - elements that make up a single transaction, where the transaction is treated a single unit that either succeeds completely or fails completely
- Consistency - transactions must take the DB from one valid state to another valid state
- Isolation - prevents one transaction from interfering with another txn
- Durability - ensures that data changes are permanent once the transaction is committed to the DB

Primary and Foregin keys are constrained
- **Entity Integrity** - primary key cannot be blank/null
- **Referential Integrity** - every foreign key actually exists as a primary key of its origin table
	- If one of the primary keys in a relationship is deleted, then all rows that reference it's foreign key are also deleted

### Structured Query Language (SQL)
- Most dominant query language for relational DBs
- Industry standard that is interoperable between database engines and programming languages

### Data Access Controls
Must properly implement/setup the following
- Authentication
- Authorization
- Audit Logging

### Data Normalization
Where information is organized efficiently and consistently before storing
- Removes dupes (reduces costs and improves data retrieval efficiency)
- Closely related fields are grouped together

### Scaling and Sharding
A data partition refers to the disk

A shard refers to a copy of a partition's schema
- Shards use same schema, which is referred to as a **horizontal partition**
- To use another shard or partition, you must add logic **outside of the database** to direct queries to the correct partition
	- This is necessary because DBs only validate the data's structure; they do not validate whether the actual data should be in this DB
- Example
	- Suppose you have two databases "Rain" and "Snow", and each have the same schema
	- If a record for "snow" gets put into the "rain" database, the database will allow it since the schema matches. However, now your apps might return incorrect data when they fetch from the "rain" database.

Because of the complexity involved with horizontal partitioning/scaling, it is preferred to do **vertical partitioning/scaling**

## Non-Relational Databases
Very popular because you don't need a strict schema, so you can start working with your database very quickly
- Very flexible and dynamic

Non-relational DBs are also known as **NoSQL databases**

Common features
- Typically open-source
- Schema-less
- Horizontally scalable
- Does not adhere to ACID constraints

Most NoSQL databases access data using some API or SDK. _However_, some NoSQL databases use a subset of SQL for data management.

### Horizontally Scalable

NoSQL Databases are designed as a **shared-nothing architecture**
- NoSQL databases often run in clusters of compute nodes
- Data is partitioned across multiple computers, so each computer can perform specific tasks independently
- Each node performs its task without having to share CPU, memory, or storage with other nodes

Scaling a non-relational database is significantly easier than scaling a relational one.

### Non-ACID
- NoSQL primarily focuses on high availability and scalability.
- As a result, consistency or durability must be sacrificed. Since durability is extremely importent, NoSQL gives up consistency because developers can design and work around it.
- **Note:** There are some NoSQL databases that support strong consistency.

### Family of NoSQL

- Key-Value - very flat and simple; can only get/put/delete a key-value pair (think of a `Map<string, object>`)
- Column Store
- Document - every document is semi-structured data that can be queried (think of a JSON object)
- Graph - data is stored via vertices and edges

## Managed NoSQL services on AWS

[See NoSQL Services in AWS](/cloud-practitioner/databases-nosql-services.md)

# Relational Database Service (RDS)

[See RDS](/databases/rds.md)
[See RDS Proxy](/databases/rds.md#rds-proxy)
[See RDS Multi-AZ](/databases/rds-multi-az.md)

# DynamoDB (DDB)

[See DDB](/databases/ddb.md)

Quick overview
- Key-value store
- Ultra-high performance
- Single digit latency
- Highly available, replicated across 3 different AZs in a region
- Downsides
	- Eventual consistency
	- Less flexible queries (tradeoff for performance)
	- Records can be max of 400kb
	- Max of 20 global indexes
	- Max of 5 secondary indexes per table
- Very easy to setup and use
	- Set tables up
	- Configure provisioned throughput or on-demand throughput
- Charged for throughput used and storage used
- Every record needs a primary key
	- Can be primitive type
	- Can be composite primary key: partition and sort key
- Secondary indexes
	- Global vs Local secondary indexes
- [Auto scaling for RCUs and WCUs](/databases/ddb.md#auto-scaling-for-readwrite-capacity-units)
- Server side encryption to protect data at rest

# Redshift

[See Redshift](/databases/redshift.md)

Quick Overview
- Based on PostgreSQL, but is not the same
- A **Redshift Cluster** contains
	- A database
	- 1 or more **compute nodes**
	- **Leader node** (only present if there are multiple compute nodes)
- Every **compute node** has its own CPU, storage, and memory
	- Different node types (RA3 or Dense)
- When queries are received from external apps, **leader node** coordinates communication between other nodes
	- Think of the **leader node as a gateway**
	- If query references tables associated with a compute node(s), then the query is distributed to those compute nodes
	- If not, then the query runs on the leader node
- Every compute node has a **node slice**, a partition of a compute node where memory and disk spaces split
	- A node slice can process queries given by leader node
	- Since compute node can have multiple slices, parallelism is leveraged
- Applications connect to your Redshift DB via ODBC or JDBC
- Highly performant
	- Parallel operations because of compute nodes and slices
	- Columnar data storage - precisely access data needed for a query, without having to scan every row
	- Caching results
- Can configure up to 10 IAM roles for a redshift cluster
- Monitoring
	- From CloudWatch
		- CPU utilization
		- Throughput
	- **Exclusively from Redshift console**
		- Query/Load performance
