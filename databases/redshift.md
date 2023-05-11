# Redshift

- Petabyte-scale data warehouse
	- **Data warehouse** is where data is consolidated from multiple sources
	- Usually stores large amounts of data
	- Undergoes **data cleansing**, where bad/invalid records or corrupted data is removed or repaired
	- Performs **ETL jobs** (extract, transform, load), which combines, parses, and transforms data from the various input sources
		- **Extraction** - collecting the data from a source
		- **Transform** - mapping the data to your needs
		- **Load** - storing the data in your warehouse
- Relational database, so it's compatible with other RDBMS apps

## Components

Redshift Cluster
- Runs a Redshift Engine
- Has 1 database
- Has at least 1 compute node
- Can have up to 10 different IAM roles

Compute Node
- There are different compute node types (RA3 and Dense)
- If there are multiple compute nodes, then one of the nodes will be a **leader node**
	- Responsible for coordinating communication between other nodes in the cluster and when external apps access this cluster
	- If a query from an external app references tables that are on the compute nodes, then leader will distribute the query out to those nodes.
		- Otherwise, the leader node will execute the query
- Split into **slices**

Node Slice
- A partition of a compute node where RAM and disk spaces split
- Slices process operations given by leader node, and can do it in parallel.
	- As a result, parallel operations are performed across all slices and all nodes for a single query
- Each compute node type has a different amount of slices/partitions that a single node can have

Network connectivity
- ODBC - open database connectivity
- JDBC (for PostgreSQL) - Java database connectivity

## Performance
- Massive Parallel Processing (MPP)
	- See parallel note in "Node Slice"
- Columnar Data Storage and Result Caching
	- Reduces # of operations on database
